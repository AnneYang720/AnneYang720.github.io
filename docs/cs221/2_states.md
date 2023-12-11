# States

## Search

- Modeling
- Tree Search
- Dynamic programming
- Uniform cost search
- A-star and its Relaxiation

### A-star
Run uniform cost search with <b>modified edge costs</b>:

${ModifiedCost(s,a)} = {Cost(s,a)} + {h(Succ(s, a)) - h(s)}$

- Heuristic function

    A heuristic $h(s)$ is any estimate of $FutureCost(s)$

- <b>relaxation</b>
    Constraints make life hard. Get rid of them. But this is just for the heuristic!


## Markov Decision Processes

- MDP: graph with states, chance nodes, transition probabilities, rewards
- Policy: A <b>policy</b> $\pi$ is a mapping from each state $s \in States$ to an action $a \in Actions(s)$
- Policy evaluation: iterative algorithm to compute value of policy, (MDP, $\pi$) $\rightarrow$ $V_\pi$
    - utility: The utility of a policy is the (discounted) sum of the rewards on the path (this is a random variable)
    - Value of policy: Let $V_\pi(s)$ be the expected utility received by following policy $\pi$ from state $s$
    - Q-value of a policy: Let $Q_\pi(s, a)$ be the expected utility of taking action $a$ from state $s$, and then following policy $\pi$
- Value iteration: MDP $\rightarrow$ $(V_\text{opt}, \pi_\text{opt})$

### Reinforcement Learning
An important distinction between solving MDPs and reinforcement learning is that the former is offline and the latter is online.

Data
${s_0}; a_1, r_1, {s_1}; a_2, r_2, {s_2}; a_3, r_3, {s_3}; \dots; a_n, r_n, {s_n}$


#### Model-based Monte Carlo
$\hat Q_{opt}(s, a) = \sum_{s'} {\hat T(s, a, s')} [{\widehat{Reward}(s, a, s')} + \gamma \hat V_{opt}(s')]$

Estimate the MDP: $T(s, a, s')$ and $Reward(s, a, s')$
- Transitions 
$\hat T(s, a, s') = \frac{\# times (s,a,s') occurs}{\# times (s,a) occurs}$

- Rewards 
$\widehat{Reward}(s, a, s') = r \text{ in } (s,a,r,s')$

#### Model-free Monte Carlo
$\hat Q_\pi(s, a) = \text{average of } u_t \text{ where } s_{t-1} = s, a_t = a$
(and $s,a$ doesn't occur in $s_0, \cdots, s_{t-2}$)

Equivalent formulation

$\blue{\eta} = \frac{1}{1 + (\text{\# updates to (s,a)})}$

1. convex combination

    On each $(s, a, u)$:

    $\hat Q_\pi(s, a) \leftarrow {(1 - \eta)} \red{\hat Q_\pi(s, a)} + {\eta} \green{u}$

2. stochastic gradient

    On each $(s, a, u)$:

    $\hat Q_\pi(s, a) \leftarrow \hat Q_\pi(s, a) - \eta [\red{\underbrace{\hat Q_\pi(s, a)}_{prediction}} - \green{\underbrace{u}_{target}}]$


#### SARSA
On each $(s, a, r, s', a\')$:

$\hat Q_\pi(s,a) \leftarrow (1-\eta) \hat Q_\pi(s,a) + \eta \green{[\underbrace{r}_{data} + \gamma \underbrace{\hat Q_\pi(s', a')}_{estimate}]}$

bootstrapping: SARSA uses estimate $\hat Q_\pi(s,a)$ instead of just raw data $u$


#### Q-learning
On each $(s, a, r, s\')$

$\hat Q_{opt}(s, a) \leftarrow (1-\eta) \red{\underbrace{\hat Q_{opt}(s, a)}_{prediction}} + \eta \green{\underbrace{(r + \gamma \hat V_{opt}(s'))}_{target}}$

$\hat V_{opt}(s\') = \purple{\max_{a' \in Actions(s')}} \hat Q_{opt}(s\', a\')$

#### Covering the unknown
1. Epsilon-greedy: balance the exploration/exploitation tradeoff
2. Function approximation: can generalize to unseen states

    Define <b>features</b> $\blue{\phi(s, a)}$ and <b>weights</b> $\blue{w}$: $\hat Q_{opt}(s, a; w) = \blue{w} \cdot \blue{\phi(s, a)}$

    Q-learning with function approximation:

    On each $(s, a, r, s')$:

    $ w \leftarrow w - \eta [\red{\underbrace{\hat Q_{opt}(s, a; w)}_\text{prediction}} - \green{\underbrace{(r + \gamma \hat V_{opt}(s\'))}_\text{target}}] \blue{\phi(s, a)}$

#### Deep reinforcement learning
just use a neural network for $\hat Q_{opt}(s, a)$

Policy gradient: train a policy $\pi(a \mid s)$ (say, a neural network) to directly maximize expected reward