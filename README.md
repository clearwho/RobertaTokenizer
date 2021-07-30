# RobertaTokenizer实现（bert4keras可用）

Roberta采用的是**byte level的BPE(BPE)**编码，需要merges.txt和vocab.json。而BERT只需要一个vocab.txt。bert4keras例子都是中文任务，roberta也只需要一个vocab.txt。因为想用bert4keras跑英文任务，所以重新改了下HuggingFace的transformer库中的tokenizer。

bpe_tokenization.py主要参考了下面三个部分

- PreTrainedTokenizer

  <[transformers.tokenization_utils — transformers 4.7.0 documentation (huggingface.co)](https://huggingface.co/transformers/_modules/transformers/tokenization_utils.html#PreTrainedTokenizer)>

- GPT2okenizer

  <[transformers.models.gpt2.tokenization_gpt2 — transformers 4.7.0 documentation (huggingface.co)](https://huggingface.co/transformers/_modules/transformers/models/gpt2/tokenization_gpt2.html#GPT2Tokenizer)>

- RobertaTokenizer

  <[transformers.models.roberta.tokenization_roberta — transformers 4.7.0 documentation (huggingface.co)](https://huggingface.co/transformers/_modules/transformers/models/roberta/tokenization_roberta.html)>

为了让bert4keras调用，改了一些函数的名字。

调用示例：

```
from bpe_tokenization import RobertaTokenizer

merges_file = '/home/lil/Model/Roberta-base/merges.txt'
vocab_file = '/home/lil/Model/Roberta-base/vocab.json'
tokenizer = RobertaTokenizer(vocab_file=vocab_file, merges_file=merges_file, lowercase=True, add_prefix_space=True)

text = "Replace me by any text you'd like."

print(tokenizer.tokenize(text))
print(tokenizer.encode(text))
```

输出：

```
['Re', 'place', 'Ġme', 'Ġby', 'Ġany', 'Ġtext', 'Ġyou', "'d", 'Ġlike', '.']
[9064, 6406, 162, 30, 143, 2788, 47, 1017, 101, 4]
```

另外修改了bert4keras中pretraining中的data_utils.py，已通过测试（BookCorpus）。

