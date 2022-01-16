# Precondition and Effect Reasoning for Action Recognition
- Currently, our paper is under the review in the journal (Computer Vision and Image Understanding)
- 
## [1] Overview
- Reinforcing the task of action recognition using annotations relevant to action.
- Annotations: precondition, effect, super-class
- By using annotations relevant to action, we show that action prediction can be improved in cycles.

## [2] Idea
- Somewhat similar to psuedo-labelling training as called somewhere else.
![alt text](https://github.com/kaiyoo/Precondition-and-Effect-Reasoning-for-Action-Recognition/img/front_page.PNG?raw=true)

- For example, if the label (action) is "Picking a cup up", generate following pseudo-labels,
> 1) Precondition: A cup is on the table
> 2) Effect: Hand is holding a cup
> 3) Super-class: pick
- And train the network originally desiged for predicting action to predict these pseudo-labels and feed their softmax probability to train action. 

## [3] Annotations
> Besided action labels provided by something-something V2 dataset, we labelled annotations (precondition, effect, super-class) relevant to action.
> 
> Annotations:
> 1) Precondition: 60 classes
> 2) Effect: 88 classes
> 3) Super-class: 23 classes
> 
> Full labels of these annotations can be seen in:
- https://docs.google.com/spreadsheets/d/1L3YeTIQuzGcXsW92mC1ALrSRtj0_g6CTsMjay0XaYS0/edit?usp=sharing



## [4] Models
For the base network and baseline, we used the code of TPN (Temporal Pyramid Network) by Yang et al.

![alt text](https://github.com/kaiyoo/Precondition-and-Effect-Reasoning-for-Action-Recognition/img/main_cycle.PNG?raw=true)
> 1) First, probabilities for each action class are computed as shown in the upper part of this figure. 
> 2) For the next step, in the bottom part of the figure, annotations are trained with action information, and probabilities for each annotation are computed and fed to train action model again. 
> 3) Step 1 and 2 form a cycle. This cycle can be repeated as long as the improvement of validation accuracy is observed.

![alt text](https://github.com/kaiyoo/Precondition-and-Effect-Reasoning-for-Action-Recognition/img/cycle_unfolded.PNG?raw=true)
> For training action, softmax probability of annotations from the very previous step and action softmax probability from the previous cycle are added. 
> For training each model, best model weight saved from the previous cycle is loaded. 

## [5] Results
- The best of our models, Model(E,P,S,A), which was trained up to Cycle2, outperforms TPN baseline by 3.4%.

Models | Cycle1 | Cycle2 | Cycle3
--- | --- | --- | ---
Model(E,P,S,A) | 65.15 | 65.40 | 65.29 | 
Model(E,P,S)  | 64.21 | 64.50 | —
Model(E,P) | 64.08 | 64.00 | —

- Action model using the following softmax prob (E:Effect, P:Precondition, S:Super-class, A: Action)
- More detailed results can be checked below. (Pretrained weights are also available.)
- https://github.com/kaiyoo/Action-and-Effect-prediction/tree/main/models

