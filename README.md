# TAAC: Temporally abstract actor-critic for continuous control

This repo contains the experiment configuration files for training TAAC on 5 categories of 14 continuous control tasks as done in the report

> TAAC: Temporally Abstract Actor-Critic for Continuous Control, Yu et al., NeurIPS 2021.

## Installation

Our experiments use the training pipelines and algorithms of [Agent Learning Framework (ALF)](https://github.com/HorizonRobotics/alf). Python3.7 is currently supported by ALF and [Virtualenv](https://virtualenv.pypa.io/en/latest/) is recommended for the installation. After activating a virtual env, download and install ALF:

```bash
git clone https://github.com/HorizonRobotics/alf
cd alf
git checkout fb30ce1 -B taac
pip install -e .
```

On top of the basic ALF installation,
- One task category **Terrain** requires installing [box2d-py](https://pypi.org/project/box2d-py/).
- Two task categories (**Manipulation** and **Locomotion**) require installing [Mujoco](http://www.mujoco.org/). Our experiments use Mujoco 2.0 and a different version might result in a different training result. So we suggest using this exact version for the reproduction purpose. Please follow the instructions at ``https://github.com/openai/mujoco-py``.
- One task category **Driving** requires installing [CARLA](https://carla.org/) and we used version [0.9.9](https://carla-releases.s3.eu-west-3.amazonaws.com/Linux/CARLA_0.9.9.tar.gz) in the experiments. Installation instructions can be found in ``<ALF_ROOT>/alf/environments/suite_carla.py``.

After the installation, clone this repo under ALF:
```bash
cd <ALF_ROOT>/alf/examples
git clone https://github.com/hnyu/taac
```

## Run experiments

To run an experiment (e.g., training TAAC on `BipedalWalker-v2`):

```bash
cd <ALF_ROOT>/alf/examples
python -m alf.bin.train --root_dir=<TRAIN_JOB_DIR> --gin_file taac/experiments/taac/taac_terrain.gin --gin_param="create_environment.env_name='BipedalWalker-v2'"
```

Then open the Tensorboard to view the training results

```bash
tensorboard --logdir=<TRAIN_JOB_DIR>
```

## Tasks
The 14 tasks can be trained by providing the corresponding environment names to the 5 gin files

|gin file                      |`create_environment.env_name`|
-------------------------------|-----------------------------|
|`<methdod>_simple_control.gin`|"MountainCarContinuous-v0"<br>"LunarLanderContinuous-v2"<br>"InvertedDoublePendulum-v2"|
|`<method>_locomotion.gin`     |"Hopper-v2"<br>"Ant-v2"<br>"Walker2d-v2"<br>"HalfCheetah-v2"|
|`<method>_terrain.gin`        |"BipedalWalker-v2"<br>"BipedalWalkerHardcore-v2"|
|`<method>_manipulation.gin`   |"FetchReach-v1"<br>"FetchPush-v1"<br>"FetchSlide-v1"<br>"FetchPickAndPlace-v1"|
|`<method>_driving.gin`        |"Town01"|

## Code reading
The entire TAAC algorithm is implemented in the file `<ALF_ROOT>/alf/algorithms/taac_algrothm.py`.

## Troubleshooting
* Sometimes running a job complains not finding [rsync](https://linux.die.net/man/1/rsync) (ALF uses rsync to backup training code), you just need to first install it and try again. Or simply append the flag `--nostore_snapshot` when launching the job.
* CARLA "Fail to start server": just give it another try.

## Reference
If you use our TAAC algortihm, please consider citing

```
@inproceedings{Yu2021TAAC,
    author={Haonan Yu and Wei Xu and Haichao Zhang},
    title={TAAC: Temporally Abstract Actor-Critic for Continuous Control},
    booktitle={NeurIPS},
    year={2021}
}
```
