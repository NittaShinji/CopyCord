# CopyCord
Visual Studioで作成した、自機の周囲8マスをコピーするコードです。


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
