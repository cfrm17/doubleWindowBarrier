# FX Double Window Double Barrier Option Model


An FX Double Window Double Barrier option can be seen as a special case of a complex barrier option, where we can have multiple single and double partial barrier (partial barrier meaning: the barrier is effective only on a subinterval of the full option term). 

We offer a hybrid (trinomial tree plus semi-analytic formulas) pricing method for the product. Currently, the model uses spot implied volatility for the first time window, and forward implied volatility for the second time window. These are Black-Scholes implied volatilities from traded vanilla European options, but, while desired, usage of different volatilities based on strike or barrier moneyness directly within the tree is difficult to achieve. 

We will start by defining Single Knock Out Partial Barrier Option, Single Knock In Barrier Option, Double Knock Out Partial Barrier Option, and Double Knock In Barrier Option contracts on whose features the Double Window Double Barrier Option contract definition is based. The exercise style is assumed European everywhere in this paper.

Assume an underlying price (FX rate in our case) process,  , an expiry date,  , and a strike price (rate),  . Assume the underlying follows the SDE below:
 
where r is the (continuously compounded) risk-free rate (domestic rate for FX options), q is the continuous dividend yield (foreign risk-free rate for FX options),  is the volatility of the underlying, and   is standard one–dimensional Brownian process. 

The Single Knock Out Partial Barrier (SKOPB) Option has one single partial knock out barrier. Let B be the single barrier and let   be the interval on which this barrier is effective.  The possible types of Single Knock Out Partial Barrier Option contract are: up-out-call/put (UOC/P), down-out-call/put (DOC/P). If the barrier interval is effective over the whole term of the option, we obtain the basic Single Knock Out Barrier (SKOB) Option contract. Define the minimum and maximum of the underlying process over a given time interval as follows:
 

The payoffs of such contracts are then defined as follows:
 
where   if call,   if put, and   if condition is true,   if condition is false.

The Single Knock In Barrier (SKIB) Option has one knock in barrier effective for the whole term of the option (we do not define here a general partial version for knock in). Let B be the single barrier. The possible types of Single Knock In Barrier Option contract are: up-in-call/put (UIC/P), down-in-call/put (DIC/P). The corresponding payoffs are then:
 

The Double Knock Out Partial Barrier (DKOPB) Option contract has two partial knock out barriers. Let H and L be the two partial knock out barriers (no order specified for H and L), and the intervals on which they are effective be   and   (they may be overlapped or disjoint). 

The possible types are: H-up-L-up-out-call/put (HULUOC/P), H-up-L-down-out-call/put (HULDOC/P), H-down-L-up-out-call/put (HDLUOC/P), and H-down-L-down-out-call/put (HDLDOC/P). If H is bigger than L, H is up, L is down, and their intervals cover the whole option term, then we obtain the basic Double Knock Out Barrier (DKOB) Option. The payoffs of such contracts are:
 

The Double Knock In Barrier (DKIB) Option contract has two total knock in barriers (we do not define here a general partial version for knock in). Let H and L be the two knock in barriers, with H bigger than L. Only two types are considered: H-up-L-down-in-call/put (HULDIC/P). The corresponding payoffs are then:
 

We can now define the Double Window Double Barrier (DWDB) Option contract. The term of the option is split into two windows. Let  be the end date of the first window. The first window,  , has the characteristics of a DKOPB  (or SKOPB, if one of the barriers is not available), that is, we have partial knock out barriers H and L with effective time intervals   and  (or just one partial knock out barrier,  , with effective time interval  ). 

The second window,  , has the characteristics of a DKOB (SKOB) or DKIB (SKIB), that is, we have total (effective over the whole second window) knock out (knock in) barriers   and  , with   (or just one total knock out (knock in) barrier for the whole second window,  ). For example, the payoff of a DWDB call with HULUOC/P characteristics for the first window, and HULDIC/P characteristics for the second window is:
  
 

that is, both the HULUOC/P condition and HULDIC/P condition, verified at option expiry date, determine the payoff. All other combinations permitted by the DWDB contract described above, can be presented in the same fashion.

There are analytic formulas for SKOB and SKIB options, and particular types of SKOPB and SKIPB options (either the start of the barrier interval is the valuation date or the end of the barrier interval is the expiry date). There are also semi-analytic formulas for DKOB and DKIB options. The DKOPB and SKOPB options can only be treated using a trinomial tree pricing. These two types of options are the main focus of this paper. We will use the above products that posses (semi) analytic formulas for testing the convergence of our lattice pricing.


•	Trinomial tree pricing for Multiple Single Partial Barrier (MSPB) Options 

The trinomial tree discretizing our underlying dynamics (1) is based on a common market practice. We parameterize our trinomial tree (shift it spatially in log scale) so that the barriers match underlying values. (In fact, it should be seen as a discretization of the Black-Scholes PDE associated to (1), thus allowing for negative “probabilities” for its branches.) 

Tree state building

Assume a number of n time steps, and assume  a series of barriers applicable at each time level. Let   be the length of the time step. Let   be the value of the underlying at time  . Let   be the spatial step in log scale, where we choose   (standard choice that assures the convergence of the standard trinomial tree). Consider the underlying value at time level   and space level 
 , denoted by  . If the underlying is in state   at time level  , the three values the underlying may take at next time level,  , are  ,  ,  . We define them as follows:
 
where   is a parameter which we will use to match barrier  . 
Equivalently, from   we construct   via
 ,		(2)
and then all the other nodes at time level   are built by 
 		(3)
The relevant states are for , but we will use relation (3) beyond this restriction, in which case we talk about unrestricted underlying values. We want our barrier   to match one of the (possibly unrestricted) underlying values at time level  , that is, for some integer   we want the following equality to hold:
 .
This is equivalent to:
  .   	(4)
Assuming we know  (and automatically all other states at time level  ), in (4) we are presented with one equation and two unknowns:  real number  , and integer  . 
We will choose first the integer   so that   comes as close as possible to an (possibly unrestricted) underlying value at time level  , assuming that the central node at this time level is equal to the one at the precedent time level, that is, the distance
 
is minimal. This minimization problem is easily solved by the integer:
 ,
where   is the integral part of the real number x (that is, the integer with the following property  ; consequently,  , which makes   the closest integer to the real number x).
Then   is chosen so that it solves exactly equation (4):
 	(5)
Note that   is very close to 1, which means the tree geometry does not change by much if the chosen mesh is dense enough. 
Given  , from (2), (3), and (5) we can build, inductively, all possible states for our underlying at all time levels.

Tree probabilities

Let   be the value of the underlying at time  . From our assumed dynamics (1) we have:
 	(6)
Let us introduce the following notation:
 
Consider now in our discretization the probabilities of state  branching off to  ,  ,  , denoted respectively (up),  (down),  (middle). Denoting by   and then using (6), we obtain the following system of equations:
 
Equivalently:
 
Solving this linear system we obtain:
 

Pricing on the tree

Now that the full trinomial tree, states and ramification probabilities, is built, we can use it to price a multiple single barrier option. Let    be the option price at time level   and space level  . Let   be the barrier condition at time level   and space level   (for example, if   is up and out type, then  ).
At time level   we have:
 
To roll backwards, we use the averaging and discounting relation together with the barrier condition:
 
The price of the option is then  .


•	Pricing SKOPB and DKOPB options

For SKOPB option pricing, all we need to do is to set   and use   instead of the above barrier condition  .

For DKOPB, we first set   and then we modify slightly  so that the other barrier, H, is matched automatically:
 ,

where  .
This new  is still very close to the old one so that the stability and convergence of the tree will not be affected, but also H gets matched by an underlying value. Indeed we have from (4)
 .
Then we want an integer  such that
 .
Subtracting we obtain:
 ,
which leads to:
 

•	Hybrid pricing for DWDB option: trinomial tree for first window, (semi) analytic formulas for second window

The idea is to use the corresponding analytic or semi-analytic formulas for the second window, where we must have a product that admits such pricing (basically, the requirement is that the barriers involved in the second window should be total, that is, being effective from the end date of the first window to the expiry date of the option). We then build a tree only for the first window, using for the nodes at the end date of the first window the (semi) analytic prices we get from the second window. One advantage is that we can use the forward volatility for the second window (this way, we include some volatility term structure in our pricing). For a generalization of this product and its pricing see https://finpricing.com/lib/EqBarrier.html

