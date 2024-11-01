# nanogpt_Chinese
## （一）简介
1. 通过 GPT2-Chinese 训练自己整理的语料。
2. 套用训练完成的语言模型，通过自定义的前导文字，进行后续的文字生成。


## （二）文件结构

- generate.py 与 train.py 分别是生成与训练的脚本。
- train_single.py 是 train.py的延伸，可以用于一个很大的单独元素列表（如训练一本斗破苍穹书）。
- eval.py 用于评估生成模型的ppl分值。
- generate_texts.py 是 generate.py 的延伸，可以以一个列表的起始关键词分别生成若干个句子并输出到文件中。
- train.json 是训练样本的格式范例，可供参考。
- cache 文件夹内包含若干BERT词表，make_vocab.py 是一个协助在一个train.json语料文件上建立词表的脚本。 vocab.txt 是原始BERT词表， vocab_all.txt 额外添加了古文词， vocab_small.txt 是小词表。
- tokenizations 文件夹内是可以选用的三种tokenizer，包括默认的Bert Tokenizer，分词版Bert Tokenizer以及BPE Tokenizer。 
- scripts 内包含了样例训练与生成脚本

## （三）train指令
```
python train.py --device=0 --epochs=10 --batch_size=2 --min_length=10 --raw_data_path=data/train.json --output_dir=model/ --raw
```
| 参数 | 说明 |
| ------ | ------ |
| `train.py` | 训练用主程序 |
| `device` | 指定用哪一个 GPU（没 GPU，默认 CPU） |
| `epochs` | 训练几回 |
| `batch_size` | 每次拿几个样本进行训练。常见的是 2 的 n 次方 |
| `min_length` | 每个样本至少需要多少长度才拿来训练 |
| `raw_data_path` | 训练数据 JSON 文件路径 |
| `output_dir` | 训练完的语言模型存放文件夹 |
| `raw` | 设置此参数，会将样本进行 tokenize |

## （四）generate指令
```
python generate.py --length=250 --nsamples=3 --prefix="若非如此" --temperature=0.7 --model_path=model/model_epoch10/ --save_samples --save_samples_path=output/
```
| 参数 | 说明 |
| ------ | ------ |
| `generate.py` | 生成文字用主程序 |
| `length` | 生成文字的长度 |
| `nsamples` | 生成几个文章范本 |
| `prefix` | 生成文章的前导文字，会影响生成的发展 |
| `temperature` | 生成温度越高，模型产生出来的结果越随机、越不可预测；换言之，使得原先容易被选到的字，抽出的机会变小，平常较少出现的字，被选到的机会稍微增加 |
| `model_path` | 生成文字所使用的语言模型资料夹路径 |
| `save_samples` | 有设置的话，会保存生成文章的范本 |
| `save_samples_path` | 生成文章范本的保存路径 |

## （五）生成文本示例

-以下为金庸小说<<射雕英雄传>>的生成样例，[model和data](https://drive.google.com/drive/folders/1x6oRuAzYG0PZ4FRrMlSShthw-9RUDby4?usp=drive_link)单独上载到云存储服务。语料 43.2MB，Batch size 2，10层深度下训练10轮所得。
![avatar](sample/散文1.png)
![avatar](sample/散文2.png)
![avatar](sample/散文3.png)
