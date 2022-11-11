# RHSS - Learning Local Rewriting from the Perspective of Group Mining

This repository contains the code used for the experiments in the paper "Learning Local Rewriting from the Perspective of Group Mining" 

During the search for solutions, RHSS repeditatly "destroys" routing problem solutions and then "repairs" them using deep neural network models with attention mechanism. The models are trained via reinforcement learning. 



## Requirements

RHSS requires python (>= 3.6) and the following python packages:

- numpy
- pytorch (we used version 1.3.1 for evaluating RHSS)

## Quick Start



### Single Instance Search

To solve the provided single instance *XE_1_seed_123_id_0.vrp* using the provided pre-trained models run the following command: 


```bash
python3 main.py --mode eval_single --model_path trained_models/cvrp/XE_1 --instance_path instances/XE_1/XE_1_seed_123_id_0.vrp --lns_nb_cpus 10 --round_distances
```



###  Batch Search

Batch search mode allows to quickly solve a set of instances in parallel. To solve the instance set *vrp20_test_seed1234.pkl* which contains 10.000 VRP instances with 20 customers and was generated using the code from [the attention model approach](https://github.com/wouterkool/attention-learn-to-route/) run:

```bash
python3 main.py --mode eval_batch --model_path trained_models/cvrp/altr_C_20 --lns_nb_cpus 10 --instance_path instances/ALTR/vrp20_test_seed1234.pkl --lns_batch_size 1000 --lns_timelimit 420 --lns_adaptive_search 
```

The search will take 420 seconds.

## Usage



### Batch Search

Batch search mode does not use a SA-based acceptance criteria. Optionally, the model selection probability can be adapted throughout the search based on the observed model performance (similar to adaptive large neighborhood search). This can be enabled by the `--lns_adaptive_search` flag.

##### Split delivery vehicle routing problem

 To solve the instances of the instance set *vrp20_test_seed1234.pkl* allowing split deliveries you can run:

```bash
python3 main.py --mode eval_batch --model_path trained_models/sdvrp/altr_SD_20 --lns_nb_cpus 10 --instance_path instances/ALTR/vrp20_test_seed1234.pkl --lns_batch_size 1000 --lns_timelimit 600 --lns_adaptive_search --allow_split_delivery
```

### Training
For example, run the following command to train a model that learns to repair instances from the *ALTR_20* instance group that have been destroyed using the tour-based destroy procedure (T) with a degree of destruction of 20%:

```
python3 main.py --mode train --instance_blueprint ALTR_20 --lns_destruction T --lns_destruction_p 0.2 --nb_batches_training_set 500 --lns_timelimit 300 --lns_batch_size 500
```



### Instance Generator 

You can generate new instances from the XE instance groups and save them as .vrp files that are  supported by other (heuristic) solvers (e.g. LKH3). For example, the following command generates 10 instances of the XE_1 instance group (using 0 as a seed) and saves them in *data/XE_1_instances*.

```
python3 generate_instances.py --data_dir data/XE_1_instances --instance_blueprint XE_1 --dataset_size 10 --seed 0
```

