# Note

> For my own convenience, I will write this note in Chinese
>
> Maybe I'll translate it to English later...

## 强化学习

> Reinforcement Learning

### Q-learning

$Q$ 表

- 行为状态 $s$，列为行为 $a$
- 值为期望
- 我们希望对于每一个状态，只要选取期望最大的行为去做，就可以取得最好的结果

算法

```
R = [...](m*n 维)
探索率 = 0.8
学习率 = 0.8
折扣率 = 0.8

Q = [0](m*n 维)

while (学习未结束):
	s = 随机状态
	while (单次训练未结束):
		a = 策略(s)
		s' = 执行(a)
		Q[s,a] = bellman(s,a,s')
		s = s'
return Q
```

- $R$：奖励表，与 $Q$ 表同维度
- 探索率：$\varepsilon$
  - 为了防止陷入局部最优：在 $s_i$ 处执行 $a_1$ 后得到正奖赏，其它动作的期望都是 0，导致以后每次处于这个状态都只会采用 $a_1$
  - 因此我们决定 $\varepsilon$ 的概率随机执行动作，$(1-\varepsilon)$ 的概率按照期望执行动作
- $bellman(s,a,s')$
  - $=(1-\alpha)*Q[s,a]+\alpha*(R[s,a]+\Upsilon*max_a Q[s',a])$
  - $\alpha$：学习率
    - 学习率越大，对之前训练效果的保留就越少
  - $\Upsilon$：折扣率
    - 折扣率越大，就越关心眼前的奖励而非记忆中将要获得的奖励

