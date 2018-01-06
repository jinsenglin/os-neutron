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
