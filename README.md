# DeepOPF
DeepOPF is a teaching simulation of adversarial attack and defense based on active load as input and active generation as output in optimal power flow

# Installation (Python)
Use Git to install DeepOPF in your current python environment

```
git clone https://github.com/GuoLIN297/DeepOPF.git
cd DeepOPF
pip3 install -r requirements.txt
python3 setup.py develop
```

# Installation with Git (Conda)
Use Git to install a conda environment for DeepOPF

```
git clone https://github.com/GuoLIN297/DeepOPF.git
cd DeepOPF
conda env create -f environment.yml
python3 setup.py develop
```

# Basic Usage
Once DeepOPF is installed, adversarial attacks and defenses on optimal power flow be generated as follows.
First, you'll need to create an object with the power system of interest

```
Nbus = '14' # Number of buses
device = torch.device("cuda:0" if torch.cuda.is_available() else "cpu")
print(device)
print("Let's use", torch.cuda.device_count(), "GPUs!")
data_path = './data/'
data = scipy.io.loadmat(data_path + 'case' + str(Nbus) + '.mat') # Dataset
epoch_training = 100
epoch_attack = 10
cost = torch.tensor([10, 20]) # The cost of unit generation for each generator
attack_method = 0
defense_method = 0
case = DeepOPF(data, epoch_training, epoch_attack, Nbus, attack_method, defense_method, cost, device)
```

Then, define the path to save the original model and train the original model

```
training_model_path = 'Training' + Nbus + '.pth'
model_original = case.train_model(training_model_path, 0)
```

Define the path to save the defensive model and train the defensive model
```
adv_defense_path = 'Defense' + Nbus + '.pth'
defense_model = case.adv_defense(model_original, adv_defense_path)
```

Finally, evaluate two models under adversarial attack
```
case.evaluate_result(model_original, defense_model)
```

We also provide a way to compare the result between diffent power systems. The result will be saved in a .csv file automatically, so you can use polt_multi_systems() to show the results
```
case.polt_multi_systems()
```
