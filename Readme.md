
# documentsvom


cd doc/

## Initialisation

pip3 install sphinx-rtd-theme
pip3 install myst_parser


## Setup

python3 -m venv .venv
source .venv/bin/activate 


make html

open ./build/index.html
