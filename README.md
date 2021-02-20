Greek language resources for [HeidelTime](https://github.com/HeidelTime/heideltime) developed using [WikiWarsEL](https://github.com/mkapernaros/WikiWarsEL) corpus for the training process and [TestWarsEL](https://github.com/mkapernaros/TestWarsEL) for the evaluation process.

For an online demo please vitit <https://heideltime-el-demo.root.sx>

Evaluation results:

| | F_Score | Precision | Recall |
|:-----|:--------------|:-----------|:------------|
| Strict Match | 85.87 | 83.37 | 88.53 |
| Relaxed Match | 94.33 | 91.58 | 97.25 |

| | Value | Type |
|:-----|:--------------|:-----------|
| Attribute F1 | 82.31 | 90.32 |

# Reproducing Evaluation Results
The following guide is an adaptation for the TestWarsEL files of the official [Reproducing Evaluation Results](https://github.com/HeidelTime/heideltime/wiki/Reproducing-Evaluation-Results) guide. It is assumed that the resources of this repository are installed in HeidelTime version 2.2.1.

## Preparation of folder structure
1. Create the following directory structure (the top folder will be referred to as `EVALPATH`):
 * `EVALPATH/`
 * `EVALPATH/corpora/`
 * `EVALPATH/evaluation_results/`
 * `EVALPATH/uima_output/`
 * `EVALPATH/uima_workflows/`

2. [Download TestWarsEL](https://github.com/mkapernaros/TestWarsEL) and extract it to `EVALPATH/corpora/`.

3. Download the official preparation and evaluation scripts archive scripts.tar.gz/.zip (see [download page](http://dbs.ifi.uni-heidelberg.de/index.php?id=form-downloads)) and extract them to EVALPATH, so that these scripts will reside in EVALPATH/scripts/.

4. Optionally, if you are going to use the scripts for debugging, run:
```
cd $EVALPATH/scripts
sed 's/ascii/utf8/g' -i te3-tools/evaluation-entities/evaluate_entities.py
```

## Preparation of corpora
You don't have to do anything in this step.

## Create UIMA workflows
Create the UIMA workflow as described in the official guide with the following parameters:
  * **Greek TestWarsEL**
  save workflow as: `EVALPATH/uima_workflows/testwarsel_workflow.xml`
    * Collection Reader: TempEval-3 Reader
      * Input Directory: `EVALPATH/corpora/TestWarsEL/untagged'
    * Analysis Engine: TreeTaggerWrapper
      * Language: `greek`
      * Annotate\_tokens: `true`
      * Annotate\_partofspeech: `true`
      * Annotate\_sentences: `true`
      * Chinese Tokenizer Path: empty
    * Analysis Engine: HeidelTime
      * Language: `greek`
      * Date: `true`
      * Time: `true`
      * Duration: `true`
      * Set: `true`
      * Type: `narratives`
      * Convert Durations: `true`
    * CAS Consumer: TempEval-3 Writer
      * Output Directory: `EVALPATH/uima_output/testwarsel`
---

# Run UIMA workflows
Go to `EVALPATH/scripts/` and run:
```
bash evaluate_corpus_tempeval3style.sh testwarsel $EVALPATH
```
The result file will be written to `EVALPATH/evaluation_results/testwarsel/evaluation_results.txt`
