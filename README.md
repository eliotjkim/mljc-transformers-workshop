# MLJC Transformers Workshop

## Set-up

1) Open codespace via Code > Codespace in upper-right corner of repo site ("Open in browser" works for sure, haven't tested in VSCode or JupyterLab yet).

   a) Note: 120 core-hours per month for free GitHub accounts: https://docs.github.com/en/billing/managing-billing-for-your-products/about-billing-for-github-codespaces

2) Create Python virtual environment named `venv`
   - `python -m venv venv`
  
3) Activate the new virtual environment `venv`
   - `source venv/bin/activate`

4) Install required python libraries from `ot_requirements.txt` file
   - `pip install -r ot_requirements.txt`
  
5. Select `venv` as python kernel for this JupyterNotebook

6) Run `pip install -r ot_requirement.txt` in codespace terminal. This may take up to 5 minutes!
   
   a) You may also be prompted to install the Jupyter kernel and Python extension. 
