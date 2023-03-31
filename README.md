# Multi-winner-Social-Choice-with-Incomplete-Preferences-analysis

<b>Article</b>- Multi-winner Social Choice with Incomplete Preferences<br>
<b>Authors</b>- Tyler Lu, University of Toronto, and Craig Boutilier, University of Toronto.<br>
<b>Published</b>- Twenty-Third International Joint Conference on Artificial Intelligence. 2013<br>

## Topic:
Following part 1 where we described the paper and presented the main result:<br>
<ol>
    <li> The greedy robust optimization algorithm with minimax regret, provides an effective solution
in polynomial time $O(nm^3K^2)$ where $K$ is the slate size, $m$ is the number of candidates, and $n$ 
        is the number of voters.</li>
    <li> The greedy-MMR algorithm provides an approximation of the optimal slate selection, with a factor of approximation being $1-\frac{1}{e^{\frac{1}{\alpha}}}$, which depends on the value of $\alpha$. The closer $\alpha$ is to 1,
the closer the approximation is to the optimal solution.</li>
    </ol>
    
In this part, we are implementing the Greedy and Optimal algorithms mentioned in the paper. <br>
We will compare the two in terms of running times and performance (MMR) as a function of the slate size - $K$ and number of candidates - $m$<br>

Furthermore, we will apply a random elicitation strategy and show that Greedy MR is non-increasing.<br>
We are planning to run the models on the Sushi dataset that consists of 5000 complete user preference rankings over 10 varieties of sushi. <br>
Another code we used to inspect the effect of $m$ on the runtime of the greedy algorithm is the Sushi dataset that consists of 5000 partial user preference rankings over 100 varieties of sushi. <br>

In order to create the partial information preferences, we will randomly choose pairwise compressions for each voter. We will later compare our results to the findings in the article.

## Definitions
<ol>
    <li>$PMR(\bar{a},\bar{w},p_i)=max_{w\in u_i(\bar{w})}PMR(u_i(\bar{a}),\left\{ w \right\},p_{i})$ <br></li>
    <li>$PMR(\bar{a},\bar{w},p)=\sum_{i\in N}^{}PMR(\bar{a},\bar{w},p_i)$<br></li>
    <li>$u_i(\bar{a})=\left\{ a\in \bar{a}:\neg \: \exists \:  a'\in \bar{a} \:\: s.t \:\: a'\succ _{i} a  \right\}$ -
Set of elements from $\bar{a}$ that are not dominated by any element in $\bar{a}$.</li>
    <li>$u_i(\bar{a})^+_w=\left\{ a\in u_i(\bar{a}):a\succ _iw \right\}$ - Set of elements from $u_i(\bar{a})$ that are dominating $w$.<br></li>
    <li>$B_i(\bar{a},w)=\left\{ b\in A:\exists a\in u_i(\bar{a})_w^+, a\succ _ib\succ _iw \right\}$ - The minimal gap between candidate $w$ and the most dominated candidate in the slate of $\bar{a}$ for a profile in $C(p_{i})$</li>
    <li>$B_{i}^{'}(a,w)=\left\{ B\in A / \bar{a} : b \ngtr _{i} w \;\; and \;\; \forall a\in u_i(\bar{a}), a \nsucc_{i} b\right\}$ - The maximal gap between candidate $w$ and the most dominated candidate in the slate of $\bar{a}$ for a profile in $C(p_{i})$</li>
     <li>$rel(\bar{a})$ - slate that contains only the candidate in $\bar{a}$ that appear in the partial profile $p_{i}$</li>
</ol>


## Optimal algorithm:
The regret-optimal slate $\bar{a}_{p}^{*}$ can be determined by first computing $PMR(\bar{a},\bar{w},p)$ for all pairs
of K-sets $\bar{a}$, $\bar{w}$, maximizing over $\bar{w}$ to determine $MR(\bar{a},p)$, then minimizing over these terms to compute $MMR(p)$.<br><br>
Here you can see the following pseudocode of the algorithm <b> we wrote</b> as we interpreted from the paper: <br>
<img src="https://user-images.githubusercontent.com/53708521/229229166-5b9a7c89-e86a-4299-aba5-e7e38aca4cb5.JPG" alt="Alternative text"  width="550" height="600"/>

### We will define: 
<ol>
    <li>$PMR(a,w,p|\bar{a})=PMR(\bar{a}\cup {a},\bar{a}\cup {w},v)$
Given $\bar{a}$ set, a is a proposed additional option, and w is an adversarial witness.</li>
    <li>$MR(a,p|\bar{a})=max_{w\in A}PMR(a,w,p|\bar{a})$</li>
    <li>$MMR(p|\bar{a})=min_{a\in A}MR(a,p|\bar{a})$</li>
    <li>$T_{i}(a,b)=\left\{ b':a\succ _ib'\succ _ib \right\}$- The minimal gap between $a$ and $b$ in partial profile $p_{i}$</li>
    
</ol>


## Greedy algorithm:
<br>
We would like to implemment the Greedy algorithm from the paper:<br>

![algorithm](https://user-images.githubusercontent.com/53708521/229230908-49f61cf7-0818-4a0b-9d58-35158c6717f2.JPG)

<br>Instead of computing the $MR(\bar{a},p)$ for each size $K$ slate $\bar{a}$ and selecting the slate that minimizes regret in the optimal algorithm (costs $O(nm^{2K+1}K^2)$ ), we are building the slate $\bar{a}$ by iterations until we get a slate of size $K$ (costs $O(nm^3 K^2)$ ).<br>
At each iteration $k\leqslant K$, we add the option with the least max regret given the prior items, to the slate.<br>

### Runtime:
<br>
As we discussed in the results in part A, the greedy robust optimization algorithm with minimax regret, provides an effective solution in polynomial time $O(nm^3 K^2)$ where $K$ is the slate size, $m$ is the number of candidates and $n$ is the number of voters.<br>

Instead of computing the $MR(\bar{a},p)$ for each size $K$ slate $\bar{a}$ and selecting the slate that minimizes regret in the optimal algorithm 
(costs $O(nm^{2K+1} K^2 )$ ), we are building the slate $\bar{a}$ by iterations until we get a slate of size $K$ (costs $O(nm^3 K^2 )$ ).<br>
At each iteration $k\leqslant K$, we add the option with the least max regret given the prior items, to the slate.<br>

<img src="https://user-images.githubusercontent.com/53708521/229232646-689f0f6a-3223-417c-bcb8-debc2636e8da.JPG" alt="Alternative text"  width="500" height="350"/>

As we can see, the average runtime increases significantly with the number of candidates m, but less dramatically
with K<br> 
This is consistent with the “quadratic in K” and “cubic in m” computational results above<br>
We can see that for small m and K, we can achieve an optimal solution in linear time.<br>

### MR  as a function of query updates:
<br>
We saw from the paper that the random elicitation strategy on the greedy algorithm (yellow line) is mostly decreasing as a function of the number of queries.<br>

<img src="https://user-images.githubusercontent.com/53708521/229233705-e75479ef-e0c7-4023-8770-567281fdfb4e.JPG" alt="Alternative text"  width="500" height="350"/>

<img src="https://user-images.githubusercontent.com/53708521/229234367-b25a07c7-ca9a-4711-a645-d14fe8fd696b.JPG" alt="Alternative text"  width="500" height="350"/>

We computed the conditional MR as a function of the number of queries added to the partial profile (over 20 randomly drawn profiles).<br>
Our location strategy randomly picks a voter and pairwise query (ensuring this query is not implied by that voter’s partial ranking), then we are using Greedy to compute a slate at each round and measure its max regret and minimax regret.<br> 
As we can see the Greedy MR may not decrease monotonically, as preference updates may “mislead” Greedy into choosing an inferior slate (by contrast, true MMR is non-increasing)<br>
It is worth mentioning that according to the paper they started with no vote information, here we start with an existing partial profile and added to it.<br>


For more information see [Multi-winner Social Choice with Incomplete Preferences Analysis.pdf]()
