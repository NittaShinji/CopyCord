# CopyCord
Visual Studioで作成した、自機の周囲8マスをコピーするコードです。

// 012
// 3 4
// 567

	//X座標の初期化
	for (int i = 0; i < 8; i++)
	{
		if (i == 0 || i == 3 || i == 5)
		{
			//上段のブロックの中心X座標を計算
			serchTransform[i].posX = object->transform.posX - (BLOCK_SIZE_R * 2);
		}
		else if (i == 2 || i == 4 || i == 7)
		{
			//下段
			serchTransform[i].posX = object->transform.posX + (BLOCK_SIZE_R * 2);
		}
		else if (i == 1 || i == 6)
		{
			//中段
			serchTransform[i].posX = object->transform.posX;
		}
	}
	//Y座標の初期化
	for (int i = 0; i < 8; i++)
	{
		if (i <= 2)
		{
			//上段のブロックの中心Y座標を計算
			serchTransform[i].posY = object->transform.posY - (BLOCK_SIZE - object->transform.radiusY) - BLOCK_SIZE_R;
		}
		else if (i >= 5)
		{
			//下段
			serchTransform[i].posY = object->transform.posY + object->transform.radiusY + BLOCK_SIZE_R;
		}
		else if (i == 3 || i == 4)
		{
			//中段
			serchTransform[i].posY = object->transform.posY - differenceRadiusY;
		}
	}

	differenceRadiusX = BLOCK_SIZE_R - object->transform.radiusX;
	differenceRadiusY = BLOCK_SIZE_R - object->transform.radiusY;

	for (int i = 0; i < 8; i++)
	{
		//右上
		serchcollisionPoint[i].rightTopX = (serchTransform[i].posX + serchTransform[i].radiusX - 1) / BLOCK_SIZE;
		serchcollisionPoint[i].rightTopY = (serchTransform[i].posY - serchTransform[i].radiusY) / BLOCK_SIZE;
		//右下
		serchcollisionPoint[i].rightBottomX = (serchTransform[i].posX + BLOCK_SIZE_R - 1) / BLOCK_SIZE;
		serchcollisionPoint[i].rightBottomY = (serchTransform[i].posY + BLOCK_SIZE_R - 1) / BLOCK_SIZE;
		//左上
		serchcollisionPoint[i].leftTopX = (serchTransform[i].posX - BLOCK_SIZE_R) / BLOCK_SIZE;
		serchcollisionPoint[i].leftTopY = (serchTransform[i].posY - BLOCK_SIZE_R) / BLOCK_SIZE;
		//左下
		serchcollisionPoint[i].leftBottomX = (serchTransform[i].posX - serchTransform[i].radiusX) / BLOCK_SIZE;
		serchcollisionPoint[i].leftBottomY = (serchTransform[i].posY + serchTransform[i].radiusY - 1) / BLOCK_SIZE;
	}

	for (int i = 0; i < 8; i++)
	{
		serchCountMapchipX[i] = serchTransform[i].posX / BLOCK_SIZE;
		serchCountMapchipY[i] = serchTransform[i].posY / BLOCK_SIZE;
	}
	for (int i = 0; i < 8; i++)
	{
		if (map[serchCountMapchipY[1]][serchCountMapchipX[1]] != NONE && map[serchCountMapchipY[3]][serchCountMapchipX[3]] != NONE && map[serchCountMapchipY[4]][serchCountMapchipX[4]] != NONE)
		{
			isMove = 0;
		}
		else if (map[serchCountMapchipY[1]][serchCountMapchipX[1]] == NONE || map[serchCountMapchipY[3]][serchCountMapchipX[3]] == NONE || map[serchCountMapchipY[4]][serchCountMapchipX[4]] == NONE)
		{
			isMove = 1;
		}

		if (map[serchCountMapchipY[i]][serchCountMapchipX[i]] == BLOCK && map[serchCountMapchipY[i]][serchCountMapchipX[i]] != NOTCOPYBLOCK)
		{
			hasReadyToCopy[i] = 1;

			if (keys[KEY_INPUT_LCONTROL] == 1)
			{
				if (keys[KEY_INPUT_C] == 1 && oldkeys[KEY_INPUT_C] == 0 && (object->transform.posY - object->transform.radiusY) < WIN_HEIGHT && commandCount >= 1)
				{
					hasCopy[i] = 1;
					isCopyed = 1;
				}
			}

			if (keys[KEY_INPUT_LCONTROL] == 1)
			{
				if (keys[KEY_INPUT_X] == 1 && oldkeys[KEY_INPUT_X] == 0 && commandCount >= 1)
				{
					hasCopy[i] = 1;
					isCopyed = 1;

					map[serchCountMapchipY[i]][serchCountMapchipX[i]] = NONE;
				}
			}
		}
		else
		{
			hasReadyToCopy[i] = 0;
		}

		if (hasCopy[0] == 0 && hasCopy[1] == 0 && hasCopy[2] == 0 && hasCopy[3] == 0 && hasCopy[4] == 0 && hasCopy[5] == 0
			&& hasCopy[6] == 0 && hasCopy[7] == 0)
		{
			isCopyed = 0;
		}
		else if (hasCopy[0] == 1 || hasCopy[1] == 1 || hasCopy[2] == 1 || hasCopy[3] == 1 || hasCopy[4] == 1 || hasCopy[5] == 1
			|| hasCopy[6] == 1 || hasCopy[7] == 1)
		{
			isCopyed = 1;
		}

		//制限回数の処理
		if (keys[KEY_INPUT_LCONTROL] == 1)
		{
			if (keys[KEY_INPUT_X] == 1 && oldkeys[KEY_INPUT_X] == 0)
			{
				commandXTimer--;
				if (commandXTimer <= 0)
				{
					commandCountFlag = 1;
				}
			}
			else if (keys[KEY_INPUT_C] == 1 && oldkeys[KEY_INPUT_C] == 0)
			{
				commandTimerToC--;
				if (commandTimerToC <= 0)
				{
					commandUseNum += 1;
					commandTimerToC = 8;
				}


				commandCTimer--;
				if (commandCTimer <= 0)
				{
					commandCountFlag = 1;
				}

				StopSoundMem(copySound);
				if (CheckSoundMem(copySound) == 0)
				{
					PlaySoundMem(copySound, DX_PLAYTYPE_BACK, true);
				}
			}
			else if (keys[KEY_INPUT_V] == 1 && oldkeys[KEY_INPUT_V] == 0)
			{
				commandVTimer--;
				if (commandVTimer <= 0)
				{
					commandCountFlag = 1;
				}

				StopSoundMem(pasteSound);
				if (CheckSoundMem(pasteSound) == 0)
				{
					PlaySoundMem(pasteSound, DX_PLAYTYPE_BACK, true);
				}
			}
		}


	if (keys[KEY_INPUT_LCONTROL] == 1)
				{
					if (keys[KEY_INPUT_V] == 1 && oldkeys[KEY_INPUT_V] == 0 && commandCount >= 1 && jumpHeight != 0)
					{
						//貼り付けた回数を数える
						commandUseNum += 1;

						//貼り付け用クールタイマーを減少
						pasteWaitTimer[i]--;

						//三方を囲んだ場合の対処
						if (hasCopy[1] == 1 || hasCopy[3] == 1 || hasCopy[4] == 1)
						{
							if (jumpHeight >= 0)
							{
								object->transform.posX = serchCountMapchipX[1] * BLOCK_SIZE + BLOCK_SIZE_R;
								object->transform.posY = serchCountMapchipY[3] * BLOCK_SIZE + BLOCK_SIZE_R;
							}
							else
							{
								object->transform.posX = serchCountMapchipX[1] * BLOCK_SIZE + BLOCK_SIZE_R;
								object->transform.posY = serchCountMapchipY[3] * BLOCK_SIZE + BLOCK_SIZE_R - 5;
							}
						}
						//各中心点をブロックサイズで割った時に、同じところにいなければペースト
						if (hasCopy[0] == 1 && object->transform.posY >= (BLOCK_SIZE * 2) + BLOCK_SIZE_R)
						{
							if (serchCountMapchipX[0] != object->centerMaptipX)
							{
								if (map[serchCountMapchipY[0]][serchCountMapchipX[0]] != NOTCOPYBLOCK)
								{
									map[object->TopYMaptip - 1][serchCountMapchipX[0]] = BLOCK;
									hasCopy[0] = 0;
								}
							}
						}
						if (hasCopy[1] == 1 && object->transform.posY >= (BLOCK_SIZE * 2) + BLOCK_SIZE_R)
						{
							if (serchCountMapchipX[1] == object->centerMaptipX)
							{
								if (map[serchCountMapchipY[1]][serchCountMapchipX[1]] != NOTCOPYBLOCK)
								{
									map[object->TopYMaptip - 1][serchCountMapchipX[1]] = BLOCK;
									hasCopy[1] = 0;
								}
							}
						}
						if (hasCopy[2] == 1 && object->transform.posY >= (BLOCK_SIZE * 2) + BLOCK_SIZE_R)
						{
							if (serchCountMapchipX[2] != object->centerMaptipX)
							{
								if (map[serchCountMapchipY[2]][serchCountMapchipX[2]] != NOTCOPYBLOCK)
								{
									map[object->TopYMaptip - 1][serchCountMapchipX[2]] = BLOCK;
									hasCopy[2] = 0;
								}
								else if (map[serchCountMapchipY[2]][serchCountMapchipX[2]] == NOTCOPYBLOCK)
								{
									map[object->TopYMaptip - 1][serchCountMapchipX[2]] = NOTCOPYBLOCK;
								}
							}
						}
						if (hasCopy[3] == 1)
						{
							if (serchCountMapchipX[3] != object->centerMaptipX)
							{
								if (map[serchCountMapchipY[3]][serchCountMapchipX[3]] != NOTCOPYBLOCK)
								{
									map[object->TopYMaptip][serchCountMapchipX[3]] = BLOCK;
									hasCopy[3] = 0;
									copyWaitTimer[3]--;
								}
							}
						}
						if (hasCopy[4] == 1)
						{
							if (serchCountMapchipX[4] != object->centerMaptipX)
							{
								if (map[serchCountMapchipY[4]][serchCountMapchipX[4]] != NOTCOPYBLOCK)
								{
									map[object->TopYMaptip][serchCountMapchipX[4]] = BLOCK;
									hasCopy[4] = 0;
									copyWaitTimer[4]--;
								}
							}
						}
						if (hasCopy[5] == 1 && object->transform.posY <= (BLOCK_SIZE * 30) + BLOCK_SIZE_R)
						{
							if (serchCountMapchipX[5] != object->centerMaptipX)
							{
								if (map[serchCountMapchipY[5]][serchCountMapchipX[5]] != NOTCOPYBLOCK)
								{
									map[object->TopYMaptip + 1][serchCountMapchipX[5]] = BLOCK;
									hasCopy[5] = 0;
								}
							}
						}
						if (hasCopy[6] == 1 && object->transform.posY <= (BLOCK_SIZE * 30) + BLOCK_SIZE_R)
						{
							if (serchCountMapchipX[6] == object->centerMaptipX)
							{
								if (map[serchCountMapchipY[6]][serchCountMapchipX[6]] != NOTCOPYBLOCK)
								{
									map[object->TopYMaptip + 1][serchCountMapchipX[6]] = BLOCK;
									hasCopy[6] = 0;
								}
							}
						}
						if (hasCopy[7] == 1 && object->transform.posY <= (BLOCK_SIZE * 30) + BLOCK_SIZE_R)
						{
							if (serchCountMapchipX[7] != object->centerMaptipX)
							{
								if (map[serchCountMapchipY[7]][serchCountMapchipX[7]] != NOTCOPYBLOCK)
								{
									map[object->TopYMaptip + 1][serchCountMapchipX[7]] = BLOCK;
									hasCopy[7] = 0;
								}
							}
						}
					}
				}
