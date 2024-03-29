# decision making in neural circuits

decision making is a cognitive process to select a choice of action/opinion, from a set of possible actions/opinions
- in essence, to make a right choice in the face of uncertainty
	- risk is inherent to interesting decisions
- choices of action are goal-directed, rather than reflexive, requiring assessment of expected consequences of each possible choice
- choices of action involve information accumulation and deliberate consideration

**questions about decision making in neural circuits:**
- If decision making is a result of collective dynamics in a neural circuit, how are the observed neural signals generated? 
- What are the properties of  a local cortical area (e.g., in the prefrontal or posterior parietal cortex) that enable it to subserve decision computations, in contrast to early sensory processing in primary sensory areas? 
- How can one establish the chain of causation linking molecules and circuits to decision behavior?

## underlying computational processes in a decision

> accumulation of evidence

- how do neurons store information acquired over a time window?

> formation of a categorical choice

- how do neurons reach a 'terminal state' whereby a final decision is represented?

> reward-based adaptation

- what are the neural learning rules (e.g. plasticity) that serve learning the reward of a decision, assuming the neural system does store some estimation of reward for alternative choices?

> stochasticity inherent in choice behaviour

- how do neurons represent the inherent randomness in the external world? what is the neural basis for random error and uncertainty?

## random dot motion discrimination task

foundational task for understanding neural contributions to perceptual decision making (Hanks & Summerfield, 2017).

A monkey maintains eye fixation while presented with an random dot kinetogram, consisting of a field of dots that is refreshed at regular intervals. 
- on each refresh, a fraction of dots is replotted with coherent motion toward a choice target 
	- rest are replotted randomly. 
	- with larger fraction (higher 'coherence'), performance improves
- monkey is rewarded for a  saccade to a target corresponding to the direction of the coherent motion. 
- task difficulty can be parametrically varied by the fraction of dots moving coherently in the same direction, called the motion strength or percent coherence

In the reaction time version of this task, monkey freely responds when it is ready.

Lateral intraparietal area is responsive to above task (variable spike density and population firing rates); sensitive to direction/decision made.

## diffusion model of perceptual decision making

consider two populations, whose average firing rates' encode a left or right decision. their firing rates increase when the relevant stimulus is received.

- circuit mechanism is unclear
- mechanism for encoding difference between population firing rates ('subtraction') unspecified

$$ \frac{d r_L}{dt} = S_L + \text{noise} $$
$$ \frac{d r_R}{dt} = S_R + \text{noise} $$
difference in population firing rates given by
$$X = r_L - r_R $$
$$\frac{dX}{dt} = S_L - S_R + \text{noise} $$
$$X(t) =  (S_L - S_R)t + \int_0^t \text{noise}\thinspace dt  $$
from above we can see $dX/dt$ can be written with a subtractive $X$ term.
$$\frac{dX}{dt} = - \frac{X}{L}+S_L - S_R + \text{noise} $$

### a potential circuit mechanism for the diffusion model

See (Wang, 2002)

![[decision making neural circuit wang.png|400]]

- Each population is selective to left or right direction
- Strong, recurrent, excitatory connections within each population
- Competition between populations introduced via inhibitory interneuron layer
- Each population receives background poisson input, and fire spontaneously at low rates.

Stimulation is generated by poisson processes to (an excitatory fraction of) each population, with rates drawn from gaussian distributions, with means $\mu_A$ and $\mu_B$ respectively (and standard deviation $\sigma$).
- for a 'left stimulus', $\mu_A > \mu_B$

Poisson rates were also varied over time, by resampling from their respective gaussian every 50 ms.

![[stimulation to decision making neural circuit wang.png|450]]

- 'coherence' $c'$ captures closeness of $\mu_A$ and $\mu_B$.
$$\mu_A = \mu_0 + \rho_Ac' $$
$$\mu_B = \mu_0 - \rho_Bc' $$
$$\rho_A = \rho_B = \mu_0/100 $$
neurons were modeled using leaky integrate-and-fire model, with neurons either
- excitatory
	- glut receptors at post; AMPA (slow deactivation) and NMDA (rapid deactivation)
- inhibitory
	- GABA receptors at post

membrane potential of a cell $V(t)$:
$$c_m \frac{dV(t)}{dt} = -g_L (V(t) - V_L) - I_{syn}(t)$$
where
- $I_{syn}$ is the total driving current arriving at the cell.

total synaptic currents given by
$$I_{syn}(t) = I_{ext,AMPA} + I_{rec, AMPA} + I_{rec,NMDA} + I _{GABA}$$

The basic idea is that population firing rates compete for dominance over the other (possible via interneurons). The above (biologically plausible) model leads to a clear winner in one population (when respective $\mu$ is higher).

![[left vs right stimulation trials in decision making circuit wang.png|450]]

- top: raster plot in both populations
- middle: population firing rates + poisson input to each population
- bottom: time integral of poisson input to each population. Correct winner can be seen by higher time integral.

note the above figure is actually with zero coherence $c'$.

consider that this model can be thought of as an attractor neural network.
- whether coherence is zero, low (12.8%), or high (51.2%), firing rates diverge dramatically over time.
	- higher contrast between inputs means that ramping slope in winner will be steeper.
- the divergent activity in the two populations is sustained after the stimulus ends; showing that the decision is 'stored'

![[decision is stored at any level of coherence wang.png|450]]

- firing rates of inhibitory interneuron population also shown at bottom

'decision space' i.e. competitive population firing rates shown below (for zero coherence)

![[zero coherence divergent population activity in decision making neural circuit wang.png|450]]

- note the initial random walk along diagonal separatrix, where firing rates are equal, prior to falling into one of two attractor states.
- right: histogram of the difference in the input time integral for trials in which the decision choice is A or B
$$ \int_0^T s_1(t) - s_2(t) \thinspace dt $$
where $s_1$ is stimulus level (poisson rate) to a winner, and $s_2$ to loser. i.e. showing you how big the difference in time integral had to be for $A$ to win, compared to when $B$ won.

the network performance (% correct over trials), for varyng coherence, is shown below.

![[decision making neural circuit performance wang.png|400]]

these curves fit Weibull functions
$$ \text{\% correct} = 1 - 0.5 \times \exp( - (c'/ \alpha)^\beta) $$
where
- $\alpha$ is the threshold coherence level for when % correct is given by 
$$ 1 - 0.5 \times \exp(-1) = 82 \% $$

reaction time simulations; i.e. time until decision is 'made' (one group reaches 15 Hz) are shown below

![[reaction time simulations in decision making circuit wang.png|450]]

- at higher coherence, faster reaction time

![[faster decisions with higher coherence wang.png|250]]

When the strength of recurrent connections is weaker, attractor dynamics can no longer be sustained by intrinsic network excitation. 

![[decision dynamics with weaker recurrent synaptic strength wang.png|350]]

- reduced ramping activity when synaptic weights at recurrent connections are weaker. neither population out-competes the other at lower coherence.
- persistent activity storing 'decision' is lost during delay period

Below, population responses are shown when the input signal is reversed (higher $\mu$ is swapped).

![[decision reversal wang.png|450]]

- left: signal strength is weak. bottom axis shows varying onset times of the reversal.
- right: if signal is reversed 1s after stimulus onset, decision is reversable by a more powerful input (higher coherence (above 70-80%)).

### optimal decision making in physical systems can often be reduced to a diffusion model

See (Bogacz et al., 2006)

this paper considers optimal decision making in binary forced-choice tasks (as above) by physical systems
- 6 models can all (except one) be reduced to the diffusion model, implementing the statistically optimal algorithm
- optimality defined by 
	- most accurate for a given speed 
	- fastest for a given accuracy 
- shows there is always an optimal trade-off between speed and accuracy that maximizes various reward functions e.g.
	- simple reward rate (percentage correct responses per unit time)
	- objective functions of reward weighted for accuracy.

## perceptual decision making in a hierarchical network

See (Wimmer et al., 2015)

![[decision making in hierarchical network wimmer.png|250]]

- sensory circuit:
	- two stimulus-selective excitatory populations
	- inhibitory population
- integration circuit:
	- two choice-associated excitatory populations
	- inhibitory population

E1 coupled to D1
E2 coupled to D2

![[response to stimulus in decision making hierarchical network wimmer.png|250]]

- response to example zero coherence stimulus. D1 and D2 separate.
- top: raster plots and population activity in D populations
- middle: raster plots and population activity in E populations.
- bottom: stimulus strength (probably meaning poisson rate again)

below, response, after top down or bottom up correlations are removed, is shown

![[bottom vs top down correlations in decision making hierarchical model wimmer.png|450]]

- see bottom (c and f) for how correlation between all pairs of excitatory populations is removed.
	- pink represents correlation within sensory populations.
	- blue represents correlation between sensory populations.
- (a and d) show population rates in each circuit, over trials yielding preferred (correct) or non-preferred choice
- (b and e) show average choice probability over time course (100 ms)
- upper energy wells showing that the stability of  the choice attractor is increased (represented by an increase in well depth) due to the bottom-up/top-down loop dynamics

## exploratory decision making

more recent studies look for more nuanced models of decision making.

**lapses in decision making reflect 'exploration'**:

See (Pisupati et al., 2021)

Perceptual decision-makers often display a constant rate of errors independent of evidence strength. 
- ‘lapses’ usually treated as arising from noise e.g. inattention or motor errors. 
	- this paper uses multisensory decision task in rats to demonstrate that above cannot account for lapses’ stimulus dependence. 
- propose lapses reflect a strategic trade-off between exploiting known rewarding actions and exploring uncertain ones. 
- selectively manipulated one action’s reward magnitude or probability.
	- changes were restricted to lapses associated with that action. 

## steps in population activity not ramps

See (Latimer et al., 2015)

Neurons in the macaque lateral Intraparietal (LIP) area exhibit firing rates that appear to  
ramp upward or downward during decision-making. These ramps are commonly assumed  
to reflect the gradual accumulation of evidence toward a decision threshold. However,  
the ramping in trial-averaged responses could instead arise from instantaneous jumps at  
different times on different trials. We examined single-trial responses in LIP using  
statistical methods for fitting and comparing latent dynamical spike-train models. We  
compared models with latent spike rates governed by either continuous diffusion-to-bound  
dynamics or discrete "stepping" dynamics. Roughly three-quarters of the choice-selective  
neurons we recorded were better described by the stepping model. Moreover, the inferred  
steps carried more information about the animal's choice than spike counts.

![[discrete steps not ramps in diffusion model of decision making.png|450]]

## alternative choices encoded by more precise neural states

See (Rich & Wallis, 2016)

When making a subjective choice, the brain must compute a value for each option and compare those values to make a decision. The orbitofrontal cortex (OFC) is critically involved in this process, but the neural mechanisms remain obscure, in part due to limitations in our ability to measure and control the internal deliberations that can alter the dynamics of the decision process. Here we tracked these dynamics by recovering temporally precise neural states from multidimensional data in OFC. During individual choices, OFC alternated between states associated with the value of two available options, with dynamics that predicted whether a subject would decide quickly or vacillate between the two alternatives. Ensembles of value-encoding neurons contributed to these states, with individual neurons shifting activity patterns as the network evaluated each option. Thus, the mechanism of subjective decision-making involves the dynamic activation of OFC states associated with each choice alternative.

![[precise orbitofrontal cortex states during decision making rich.png|450]]

- trained a linear discriminant analysis (LDA) algorithm (above shows 3) to classify single-picture trials by identifying neural patterns associated with four categories of subjective value, ranging from least to most desirable (values 1 to 4).
- shows that OFC state encodes 'certainty' about a decision. hints that OFC is 'deliberating'.