# HandyRL

![](https://github.com/DeNA/HandyRL/workflows/pytest/badge.svg?branch=master)

**HandyRL is a handy and simple framework for distributed reinforcement learning that is applicable to your own environments.**


# Quick Start Easy to Win

*   Prepare your own environment
*   Let’s start large-scale distributed training
*   Get your great model!

## How to use


### Step 0: Install dependencies

HandyRL supports Python3.7+.
You need to install additional libraries (e.g. numpy, pytorch).
```
pip3 install -r requirements.txt
```


### Step 1: Set up configuration

Set `config.yaml` for your training configuration.
If you run a training with TicTacToe and batch size 64, set like the following:


```
env_args:
    env: 'TicTacToe'
    source: 'handyrl.envs.tictactoe'

train_args:
    ...
    batch_size: 64
    ...
```

NOTE: TicTacToe is used as a default game. [Here is the list of games](https://github.com/DeNA/HandyRL/tree/master/handyrl/environments). When you use your own environment, set the name of the environment to `env` and script path to `source`.


### Step 2: Train!


```
python main.py --train
```

NOTE: Trained model is saved in `models` folder.


### Step 3: Evaluate

After training, you can evaluate the model against any models. The below code runs the evaluation for 100 games with 4 processes.


```
python main.py --eval models/1.pth 100 4
```


NOTE: Default opponent AI is random agent implemented in `evaluation.py`. You can change the agent with any of your agents.


## Distributed Training!

HandyRL allows you to learn a model remotely on a large scale.


### Step 1: Remote configuration

If you will use remote machines as worker clients(actors), you need to set server(learner) address configuration in each client:


```
entry_args:
    remote_host: '127.0.0.1'  # Set training server address to be connected from worker
    num_gather: 2
    num_process: 6
```


NOTE: When you train a model on cloud(e.g. GCP, AWS), the internal/external IP can be set here.


### Step 2: Start training server


```
python main.py --train-server
```


NOTE: The server listens to connections from workers. The trained models are saved in `models` folder.


### Step 3: Start workers

After starting the training server, you can start the workers for data generation and evaluation.


```
python main.py --worker
```



### Step 4: Evaluate

After training, you can evaluate the model against any models. The below code runs the evaluation for 100 games with 4 processes.


```
python main.py --eval models/1.pth 100 4
```



## Custom environment

Write a wrapper class named `Environment` following the format of the sample environments.
The kind of your games are:
* turn-based game: see `handyrl/envs/tictactoe.py`, `handyrl/envs/geister.py`
* simultaneous game: see `kaggle/geese.py`

To see all methods of environment, check [environment.py](https://github.com/DeNA/HandyRL/blob/master/handyrl/environment.py).


## Use case

*   [The 5th solution in Google Research Football with Manchester City F.C.](https://www.kaggle.com/c/google-football/discussion/203412) (Kaggle)
*   [Baseline RL AI in Hungry Geese](https://www.kaggle.com/yuricat/smart-geese-trained-by-reinforcement-learning) (Kaggle)