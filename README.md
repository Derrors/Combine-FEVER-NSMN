# Combine-FEVER-NSMN
Fork from: https://github.com/easonnie/combine-FEVER-NSMN.git   
The implementation for the paper：[Combining Fact Extraction and Verification with Neural Semantic Matching Networks](https://arxiv.org/abs/1811.07039) (AAAI 2019 and EMNLP-FEVER Shared Task Rank-1 System).


## Requirement
* Python 3.6
* pytorch 0.4.1
* allennlp 0.7.1
* sqlitedict
* wget
* flashtext
* pexpect
* fire
* inflection


## Preparation
1. 首先完成安装以上模块.
2. 运行脚本.
```bash
source setup.sh               # 配置项目路径及相关文件路径
bash ./scripts/prepare.sh     # 相关数据文件下载
```
注：运行脚本若无法正常下载数据及文件，可打开以下链接进行手动下载(需VPN),下载成功后按照项目结构来移动文件到相应的路径.
```bash
https://s3-eu-west-1.amazonaws.com/fever.public/ shared_task_dev.jsonl    # shared_task_dev.jsonl
https://s3-eu-west-1.amazonaws.com/fever.public/train.jsonl               # train.jsonl
https://s3-eu-west-1.amazonaws.com/fever.public/shared_task_test.jsonl    # shared_task_test.jsonl
https://s3-eu-west-1.amazonaws.com/fever.public/wiki-pages.zip            # wiki-pages.zip
https://www.dropbox.com/s/74uc24un1eoqwch/dep_packages.zip?dl=0           # dep_packages.zip
https://www.dropbox.com/s/yrecf582rqtgke0/aux_file.zip?dl=0               # aux_file.zip
https://www.dropbox.com/s/rc3zbq8cefhcckg/saved_nli_m.zip?dl=0            # saved_nli_m.zip
https://www.dropbox.com/s/hj4zv3k5lzek9yr/nn_doc_selector.zip?dl=0        # nn_doc_selector.zip
https://www.dropbox.com/s/56tadhfti1zolnz/saved_sselector.zip?dl=0        # saved_sselector.zip
https://www.dropbox.com/s/pu3h5xc2kpws0n2/chaonan99.zip?dl=0              # chaonan99.zip
```
3. 标记化数据集并建立 Wiki 文档数据库，方便之后快速地提取和查询数据
```bash
python src/pipeline/prepare_data.py tokenization        # Tokenization
python src/pipeline/prepare_data.py build_database      # Build document database. (This might take a while)
```
   
准备工作完成后,整个项目为文件结构如下：  
```bash
data
├── fever
│   ├── license.html
│   ├── shared_task_dev.jsonl
│   ├── shared_task_test.jsonl
│   └── train.jsonl
├── fever.db
├── id_dict.jsonl
├── license.html
├── sentence_tokens.json
├── tokenized_doc_id.json
├── tokenized_fever
│   ├── shared_task_dev.jsonl
│   └── train.jsonl
├── vocab_cache
│   └── nli_basic
│       ├── labels.txt
│       ├── non_padded_namespaces.txt
│       ├── tokens.txt
│       ├── unk_count_namespaces.txt
│       └── weights
│           └── glove.840B.300d
├── wiki-pages
│   ├── wiki-001.jsonl
│   ├── ... ...
│   └── wiki-109.jsonl
└── wn_feature_p
    ├── ant_dict
    ├── em_dict
    ├── em_lemmas_dict
    ├── hyper_lvl_dict
    ├── hypernym_stems_dict
    ├── hypo_lvl_dict
    └── hyponym_stems_dict

dep_packages
├── DrQA
└── stanford-corenlp-full-2017-06-09

results
└── chaonan99

saved_models
├── saved_nli_m
├── nn_doc_selector
└── saved_sselector
```

## Automatic Pipeline System.
-在 dev 数据集上运行 pipeline system：
```bash
python src/pipeline/auto_pipeline.py
```
Note that this pipeline is the (SotA) model in the AAAI paper.    

-For EMNLP-FEVER Shared Task version：.
```bash
python src/nli/mesim_wn_simi_v1_3.py
python src/pipeline/pipeline_process.py
```

## Citation
If you find this implementation helpful, please consider citing:
```
@inproceedings{nie2019combining,
  title={Combining Fact Extraction and Verification with Neural Semantic Matching Networks},
  author={Yixin Nie and Haonan Chen and Mohit Bansal},
  booktitle={Association for the Advancement of Artificial Intelligence ({AAAI})},
  year={2019}
}
```