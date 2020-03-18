# SMILES Pair Encoding (SmilesPE).
> SMILES Pair Encoding (SmilesPE) trains a substructure tokenizer from a large set of SMILES strings (e.g., ChEMBL) based on [byte-pair-encoding (BPE)](https://www.aclweb.org/anthology/P16-1162/).


## Overview

## Installation

`pip install SmilesPE`

## Examples

### Basic Tokenizers

1. Atom-level Tokenizer

```python
from SmilesPE.pretokenizer import atomwise_tokenizer

smi = 'CC[N+](C)(C)Cc1ccccc1Br'
toks = atomwise_tokenizer(smi)
print(toks)
```

    ['C', 'C', '[N+]', '(', 'C', ')', '(', 'C', ')', 'C', 'c', '1', 'c', 'c', 'c', 'c', 'c', '1', 'Br']


2. K-mer Tokenzier

```python
from SmilesPE.pretokenizer import kmer_tokenizer

smi = 'CC[N+](C)(C)Cc1ccccc1Br'
toks = kmer_tokenizer(smi, ngram=4)
print(toks)
```

    ['CC[N+](', 'C[N+](C', '[N+](C)', '(C)(', 'C)(C', ')(C)', '(C)C', 'C)Cc', ')Cc1', 'Cc1c', 'c1cc', '1ccc', 'cccc', 'cccc', 'ccc1', 'cc1Br']


The basic tokenizers are also compatible with [SELFIES](https://github.com/aspuru-guzik-group/selfies) and [DeepSMILES](https://github.com/baoilleach/deepsmiles). Package installations are required.

Example of SELFIES

```python
import selfies
smi = 'CC[N+](C)(C)Cc1ccccc1Br'
sel = selfies.encoder(smi)
print(f'SELFIES string: {sel}')
```

    SELFIES string: [C][C][N+][Branch1_2][epsilon][C][Branch1_3][epsilon][C][C][c][c][c][c][c][c][Ring1][Branch1_1][Br]


```python
toks = atomwise_tokenizer(sel)
print(toks)
```

    ['[C]', '[C]', '[N+]', '[Branch1_2]', '[epsilon]', '[C]', '[Branch1_3]', '[epsilon]', '[C]', '[C]', '[c]', '[c]', '[c]', '[c]', '[c]', '[c]', '[Ring1]', '[Branch1_1]', '[Br]']


```python
toks = kmer_tokenizer(sel, ngram=4)
print(toks)
```

    ['[C][C][N+][Branch1_2]', '[C][N+][Branch1_2][epsilon]', '[N+][Branch1_2][epsilon][C]', '[Branch1_2][epsilon][C][Branch1_3]', '[epsilon][C][Branch1_3][epsilon]', '[C][Branch1_3][epsilon][C]', '[Branch1_3][epsilon][C][C]', '[epsilon][C][C][c]', '[C][C][c][c]', '[C][c][c][c]', '[c][c][c][c]', '[c][c][c][c]', '[c][c][c][c]', '[c][c][c][Ring1]', '[c][c][Ring1][Branch1_1]', '[c][Ring1][Branch1_1][Br]']


Example of DeepSMILES

```python
import deepsmiles
converter = deepsmiles.Converter(rings=True, branches=True)
smi = 'CC[N+](C)(C)Cc1ccccc1Br'
deepsmi = converter.encode(smi)
print(f'SELFIES string: {deepsmi}')
```

    SELFIES string: CC[N+]C)C)Ccccccc6Br


```python
toks = atomwise_tokenizer(deepsmi)
print(toks)
```

    ['C', 'C', '[N+]', 'C', ')', 'C', ')', 'C', 'c', 'c', 'c', 'c', 'c', 'c', '6', 'Br']


```python
toks = kmer_tokenizer(deepsmi, ngram=4)
print(toks)
```

    ['CC[N+]C', 'C[N+]C)', '[N+]C)C', 'C)C)', ')C)C', 'C)Cc', ')Ccc', 'Cccc', 'cccc', 'cccc', 'cccc', 'ccc6', 'cc6Br']


### Use the Pre-trained SmilesPE Tokenizer

### Train a SmilesPE Tokenizer with a Custom Dataset
