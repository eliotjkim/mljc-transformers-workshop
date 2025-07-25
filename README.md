# MLJC Transformers Workshop

## Set-up

1) Open codespace via Code > Codespace in upper-right corner of repo site ("Open in browser" works for sure, haven't tested in VSCode or JupyterLab yet).

   a) Note: 120 core-hours per month for free GitHub accounts: https://docs.github.com/en/billing/managing-billing-for-your-products/about-billing-for-github-codespaces

2) **[OPTIONAL] Ignore this step if you are using code space with python 3.12 installed.** 
   - If you want to run your code on your own device with `conda` installed, create a new virtual environment with python 3.12 installed
   - `conda create --name py312 python=3.12`
   - `conda activate py312`

3) Create Python virtual environment named `venv`
   - `python -m venv venv`
   
4) Activate the new virtual environment `venv`
   - `source venv/bin/activate`

5) Install required python libraries from `ot_requirements.txt` file. This may take up to 5 minutes!
   - `pip install -r ot_requirements.txt`
  
6) Select `venv` as python kernel for this JupyterNotebook
   -  You may also be prompted to install the Jupyter kernel and Python extension. 
  
## Ozone Transformer Example Thoughts

- `torch.nn.Transformer` contain everything you need for encoder-decoder transformer. 
  - It doesn't have the tokenizer and positional embedding, but adding a `torch.nn.Embedding` (a trainable embedding) layer will be sufficient.
  - The sine-cosine positional embedding is also very good
  - It doesn't have the `softmax` layer shown in the "Attention is all you need" paper
- Embedding size (d) in transformer must be divisible by the number of head (h)
- `torch.nn.Transformer` doesn't enable any mask by default. Users have to provide their own attention mask. `Transformer` class contains a static method that generates sequential mask: `torch.nn.Transformer.generate_square_subsequent_mask`
  - `src_mask`: The attention mask on the encoder side. 
  - `tgt_mask`: The first attention mask on the decoder side.
    - This one is all you need for most of the time, to achieve the same structure as "Attention is all you need" paper
  - `memory_mask`: The first attention mask on the decoder side in the cross-attention block.
- In our example, we shifted the sample manually on the `def forward()` function. We are not sure if it's standard practice. 

## Ozone Dataset and Training Details

`data/mljc_workshop_o3_L25.nc` 
- 2019 global GEOS-Chem ozone concentrations (mol / mol) 
- 3-hourly time-steps, $4^o$ by $5^o$ resolution
- Model Level 25 (~350-400 hPa)

**Training Task:** Predict ozone concentration patch (11 by 12 grid cells, 24 total global patches) for the next time-step given N prior patches. Each input sequence of patches includes 100 time-steps (~12.5 days). 
- On GitHub Codespace with 2-core CPU, each epoch takes 10-15 seconds for a batch size of 64.

Disclaimer: no domain knowledge was applied in the making of this training dataset. 