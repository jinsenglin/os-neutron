# VENV

create python virtual environments

```
mkdir venvs

virtualenv venvs/neutron
virtualenv venvs/neutron-lbaas
virtualenv venvs/neutron-fwaas

source venvs/neutron/bin/activate
pip install -r neutron/requirements.txt

source venvs/neutron-lbaas/bin/activate
pip install -r neutron-lbaas/requirements.txt

source venvs/neutron-fwaas/bin/activate
pip install -r neutron-fwaas/requirements.txt
```

# PYCHARM

create pycharm projects

```
* project location: neutron (instead of neutron/neutron)
* python interpreter location: venvs/neutron/bin/python2.7

* project location: neutron-lbaas (instead of neutron-lbaas/neutron-lbaas)
* python interpreter location: venvs/neutron-lbaas/bin/python2.7

* project location: neutron-fwaas (instead of neutron-fwaas/neutron-fwaas)
* python interpreter location: venvs/neutron-fwaas/bin/python2.7
```
