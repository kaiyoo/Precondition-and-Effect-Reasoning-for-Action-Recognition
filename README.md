# Precondition and Effect Reasoning for Action Recognition
Currently, our paper is under the review in the journal (Computer Vision and Image Understanding).

Paper and code will be soon available after publication.

## [1] Overview
- Reinforcing the task of action recognition using annotations relevant to action.
- Annotations: precondition, effect, super-class(category of action)
- By utilizing these generated annotations, we show that action prediction can be improved in cycles.

## [2] Idea
Somewhat similar to psuedo-labelling training as called elsewhere recently.

![alt text](https://github.com/kaiyoo/Precondition-and-Effect-Reasoning-for-Action-Recognition/blob/main/img/front_page.PNG?raw=true)

- For example, if the label (action) is "Picking a cup up", generate the following pseudo-labels,
> 1) Precondition: A cup is on the table
> 2) Effect: Hand is holding a cup
> 3) Super-class: Pick
- Train the network originally desiged for predicting action to predict these pseudo-labels, and feed their softmax probability to train action. 

## [3] Annotations
- Based on action labels provided by something-something V2 dataset, we labelled following annotations relevant to action (precondition, effect, super-class).
 
> Annotations:
> 1) Precondition: 60 classes
> 2) Effect: 88 classes
> 3) Super-class: 23 classes
>

Full labels of these annotations can be seen [here](https://docs.google.com/spreadsheets/d/1L3YeTIQuzGcXsW92mC1ALrSRtj0_g6CTsMjay0XaYS0/edit?usp=sharing)



## [4] Models
For the base network and baseline, we used the code of TPN (Temporal Pyramid Network) by Yang et al.

[Cycles for training action and annotation]
![alt text](https://github.com/kaiyoo/Precondition-and-Effect-Reasoning-for-Action-Recognition/blob/main/img/main_cycle.PNG?raw=true)
> 1) First, probabilities for each action class are computed as shown in the upper part of this figure. 
> 2) For the next step, in the bottom part of the figure, annotations are trained with action information, and probabilities for each annotation are computed and fed to train action model again. 
> 3) Step 1 and 2 form a cycle. This cycle can be repeated as long as the improvement of validation accuracy is observed.

#

[Cycles unfolded]
![alt text](https://github.com/kaiyoo/Precondition-and-Effect-Reasoning-for-Action-Recognition/blob/main/img/cycle_unfolded.PNG?raw=true)
> For training action, softmax probability of annotations from the very previous step and action softmax probability from the previous cycle are added. 
> For training each model, best model weight saved from the previous cycle is loaded. (transfer learning) 


## [5] Results
- The best of our models, Model(E,P,S,A), which was trained up to Cycle2, outperforms TPN baseline by 3.4%.

Models | Cycle1 | Cycle2 | Cycle3
--- | --- | --- | ---
<b>Model(E,P,S,A)</b>  | 65.15 | <b>65.40</b>  | 65.29 | 
Model(E,P,S)  | 64.21 | 64.50 | —
Model(E,P) | 64.08 | 64.00 | —

- Model names in "Models" tab mean: Action model that uses the following softmax prob (E:Effect, P:Precondition, S:Super-class, A: Action)
- More detailed results as well as downloadable pretrained weights are available in [/models](/models).

