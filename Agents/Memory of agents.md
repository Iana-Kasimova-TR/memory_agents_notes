# Memory
 - *training memory* - knowledge and facts that a model learns during the pre-training process. This information stored in model's parameters
 - *short term memory* - temporary information, that AI posses during task executon (zero-shot, few-shot?), information about intermediate results
 - *long term memory* - information, stored in an external storage system (RAG?)
 ![[Screenshot 2024-01-28 at 15.46.45.png]]
## KAFT
By incorporating counterfactual and irrelevant contexts to standard supervised dataset. *Is it possible to design a mechanism to ensure that the context can influence the model's working memory in a desirable manner?* 

relevant context  > model’s pretrained world knowledge > irrelevant context
(> order of priority for LLM) - principle of Controllability and robustness

r = 1 : M′(c + q) = a
r = 0 : M′(c + q) = M(q) 

where r - whether the context entails the answer, a-answer from M model(before fine tune), M' - fine tuned model, c - context, q - query
model doesn't now r priori

## Voyager:
![[Screenshot 2024-01-28 at 17.11.44.png]]
key modules:
1. Automatic curriculum that maximizes exploration
2. Skill library for storing and retrieving comlplex behaviours
3. New iterative prompting mechanism that generates executable code for embodied control:
	1.  executes the generated program to obtain observations from the Minecraft simulation (such as inventory listing and nearby creatures) and error trace from the code interpreter 
	2. incorporates the feedback into GPT-4’s prompt for another round of code refinement
	3. repeats the process until a self-verification module confirms the task completion, at which point we commit the program to the skill library (e.g., craftStoneShovel() and combatZombieWithSword()) and query the automatic curriculum for the next milestone

Build cirrculum process:
	By prompting to GPT-4 following components:
		- Directives encouraging diverse behaviors and imposing constraints
		- agent's current state
		- previously completed and failed tasks
		- additional context
	
Build Skill library:
	By prompting next components:
		- Guidelines for code generation
		- Control primitive APIs, and relevant skills
		- The generated code from the last round, environment feedback, execution errors, and critique
		- The agent’s current state
		- Chain of thought prompting
	![[Screenshot 2024-01-28 at 17.19.49.png]]

**As I understand Voyager build knowledgebase for RAG automatically, and it is possible because we can get feedback from environment(code won't be executed)**

## Needle in haystack
'этого часто используют Needle in a Haystack Test, но он ничего не говорит про модель мира. Только про то, может ли модель распределить аттеншн чем-то вроде дельта-функции в произвольную точку в последовательности.' - если можно, я бы расспросила этот момент, не совсем поняла
## Clembench

- as I understand, these set of linguistic games, where we set a prompts, game flow via Game master and evaluate intermediate and final result?
- For example Taboo game - master monitors that answer from agents don't contain taboo words. By trying to build explanation of some word/concept, we can check the world and language model - here as I got that world model it is a knowledge about world in agent? 
- How we can evolve it? Maybe add number of iterations in a game? maybe repeat game, which was failed, but put in context or in knowledgebase the logs about failed previous game?

## AgentBoard - text based agent evaluation benchmark
 Task diversity, multi-round interaction(mimic realistic scenarious), partially-observable environments
Interaction with these environments can be modeled as a special case of Partially Observable Markov Decision Process -  $⟨g, S, A, O, T ⟩$, 0. with goal g, state space S, valid actions space A, observation space (including environment feedback) O, transition function $T :S×A→S$ An agent with policy $π$ makes prediction at timestep $t$ based on goal $g$ and memory $m_t = {o_j , a_j , o_{j+1}, a_{j+1}, . . . o_t}, 0 ≤ j < t$, which is a sequence of actions and observations. This trajectory of the agent $τ = [s_0, a_0, s_1, a_1, . . . s_t]$ is formulated by policy and environmental state transitions, such as
 $p_π(τ) = p(s_0)π(a_t|g,s_t,m_t)T(s_{t+1}|s_t,a_t)$

### Progress rate
In each round of interaction, a progress rate, denoted as rt, is assigned to evaluate the agent’s ad- vancement towards the goal state g. As the agent moves through the states st = [s0, . . . , st], we assess its progress using a matching score f(·,g) → [0,1] that quantifies the similarity between the current state and the goal state. The initial value of rt is set to 0, indicating no progress. The progress rate rt reflects the highest matching score achieved, reaching 1 when the task is completed. The progress rate is formulated as below:

if we have direct state comparison it is just measure similarity, bit if it is not, then we divide a goal on sequential subgoals. Each subgoal is associated with a labeled state, that indicates its completion

**As I understand the each task for each environment were annotated by subgoals by annotators(and they have a set of iterations for annotators) so that to improve quality of annotation for subgoals**

Ability to engage in multi round interactions - 
We analyze how the models proceed across long-range interactions. Specifically, we calculate the progress rate relative to the number of interaction steps



