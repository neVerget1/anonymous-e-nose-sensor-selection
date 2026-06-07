# Experimental Settings

This file provides supplementary experimental settings for the submitted paper on compact electronic nose sensor selection. The purpose is to provide additional details that cannot be fully included in the short paper due to space limitations.

## 1. Task Definition

The task is compact sensor subset selection for electronic nose multivariate time series. Given a full sensor array, the goal is to select a smaller subset of sensors while preserving the full-array classification performance.

The study focuses on sensor selection rather than designing a stronger downstream classifier.

## 2. Datasets

We evaluate the method on five electronic nose datasets:

| Short Name     | Full Dataset Name                                   | Number of Sensors |
| -------------- | --------------------------------------------------- | ----------------: |
| Low-conc.      | Gas sensor array low-concentration                  |                10 |
| Dynamic        | Gas sensor array under dynamic gas mixtures         |                16 |
| Open-sampling  | Gas sensor arrays in open sampling settings         |                72 |
| Gas10          | 10-class industrial pollution gas detection dataset |                15 |
| Chinese Liquor | Chinese liquor E-nose dataset                       |                10 |

## 3. Evaluation Protocol

All experiments follow a five-fold evaluation protocol. For model training, we use six random seeds:

```text
40, 41, 42, 43, 44, 45
```

The reported results are averaged across folds and seeds.

To avoid validation leakage, sensor ranking and subset selection are performed within the training side of each split. The held-out test fold is used only for final evaluation.

## 4. Downstream Classifier

The default downstream classifier is ResNet1D. It is used to evaluate both the full sensor array and the selected sensor subsets.

Additional classifier sensitivity experiments are conducted with TCN and InceptionTime on Gas10.

## 5. MOMENT-R3S Selection Pipeline

MOMENT-R3S uses a pre-trained time-series representation model to obtain sensor-wise representations. The MOMENT parameters are not updated during sensor selection.

Each sensor is scored by jointly considering:

1. task relevance;
2. class separability based on pairwise class centroid distance;
3. single-sensor utility;
4. prediction-level redundancy.

A redundancy-aware greedy ranking is then used to produce an ordered sensor list. The final compact subset is determined by a performance retention constraint.

## 6. Retention-Constrained Auto-k Setting

For the main results, MOMENT-R3S automatically determines the selected sensor budget.

The selected budget is defined as the smallest top-k prefix whose validation performance satisfies the retention constraint relative to the full-sensor performance.

The retention metric is computed as:

```text
Retention = Selected Macro-F1 / Full Macro-F1
```

The performance drop is computed as:

```text
Drop = Full Macro-F1 - Selected Macro-F1
```

The sensor reduction ratio is computed as:

```text
Reduction = 1 - (#Selected Sensors / #Total Sensors)
```

## 7. Fixed-Budget Baseline Comparison

For controlled comparison with baseline sensor selection methods, we use a fixed-budget setting. In this setting, all methods select the same number of sensors for each dataset.

The fixed-budget comparison is conducted on three representative datasets:

| Dataset        | Fixed Budget |
| -------------- | -----------: |
| Dynamic        |         8/16 |
| Gas10          |         9/15 |
| Chinese Liquor |         6/10 |

This setting is used to compare ranking quality under the same selected sensor budget.

## 8. Baselines

The following baseline methods are compared under the fixed-budget setting:

* TSelect;
* ECP;
* ECS;
* mutual information;
* mRMR.

All methods are evaluated under the same downstream classifier, fold split, and training-seed protocol.

## 9. Main Auto-k Results

| Dataset        | #Sensors | Reduction | Full Macro-F1 | Selected Macro-F1 |   Drop | Retention |
| -------------- | -------: | --------: | ------------: | ----------------: | -----: | --------: |
| Low-conc.      |   4.6/10 |     54.0% |        0.9591 |            0.9218 | 0.0373 |    0.9611 |
| Dynamic        |     8/16 |     50.0% |        0.9373 |            0.9242 | 0.0131 |    0.9860 |
| Open-sampling  |    40/72 |     44.4% |        0.8499 |            0.8126 | 0.0373 |    0.9561 |
| Gas10          |     9/15 |     40.0% |        0.9872 |            0.9758 | 0.0114 |    0.9885 |
| Chinese Liquor |     6/10 |     40.0% |        0.9986 |            0.9915 | 0.0071 |    0.9929 |

## 10. Notes for Double-Anonymous Review

This repository is prepared for double-anonymous review. It does not include author names, affiliations, email addresses, personal paths, or institution-specific information.
