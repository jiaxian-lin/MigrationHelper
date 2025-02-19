# Evaluation

This folder contains all the data used for evaluation. 

`manual` contains important hand-curated data. `pic/` contains data and script to draw plots in the paper. `deprecated/` contains some prior files that are not used for now. 

Recall that our goal is two-fold:
1. to mine real migrations from existing development histories
2. to take advantage of such migration instances for recommending migration targets, along with potentially useful metrics.

In this sense, our first key evaluation should be understanding the "capacity" of goal 1, 
i.e. how many real migrations we can really discover from the existing development histories (RQ1)?

If we can find such a representative set, we can also retrieve all other candidates in these set, 
then compute a lot of metrics from them and find which one is most useful,
forming RQ2: For each metric, how effective it is in identifying these real migrations?

Then, we evaluate our algorithm (using metrics from RQ2) using Precision@k, Recall@k, 
Normalized Cumaltive Distributed Gain (NDCG), and Mean Reciprocal Rank (MRR).
We also explain how parameters affect the performance of our approach, and prove that our algorithm is using the best parameters.
This forms our RQ3: How good is our algorithm in discovering these migration rules?

Finally, with the effectiveness of our algorithm verified, we run our algorithm on an extended ultra-large dataset, 
to see how many additional rules we can mine compared with [1].
This is also for validating the generality of our approach, and for additional dataset contribution.
This forms our RQ4: How many additional migration rules can we discover, compared with [1]?

Then we detail on how we plan to answer each research question.

## RQ1: How many real migrations we can really discover from the existing development histories?

[1] provides a basic list of possible Java library migrations rules mined from a large set of open source repositories and verified by experts (raw data available in `test-ground-truth-2014-raw.csv`). However, they only provide library names but not their group IDs and artifact IDs.
Therefore, we apply the following processing steps to generate a set of possible migration rules.

1. Manually map them back to possible group IDs and artifact IDs, resulting in `test-ground-truth-2014.csv`.
2. Filter out libraries that is rarely used in our test data (This is done when we evaluate, but the data still contains all libraries).
3. If one rule item maps to multiple group IDs and artifact IDs, we consider their cartesian product as all valid rules.
4. For the remaining rules, we use the following heuristics to extend it: if (A, B) is a valid rule and (B, C) is a valid rule, then (A, C) is also an valid rule. (Not sure if this is needed, currently not implemented because the data are too many)

All those process steps might generate wrong rules. 
To make things worse, it's very hard to manually validate whether a rule is really a migration rule without a Java expert in that domain.
Therefore, we take a very conservative approach.
For each possible ground truth rules above, we analyze the number of possible occurrences in the data, and find its relavant commits (209356 commits).
Then, we design a set of heuristics to find commits that states their migration in the commit message, and manually validate the results.
The heuristics are designed from a manual inspection of 6000 stratifid samples in which we discover that only xxx samples states explicitly it is doing migration in the commit message.
For all the rules, only those that we can find at least one real migration will be kept as evaluation set?(Or we still keep the old rules with a lower confidence)

During the manual inspection, we also have the following observations:
1. Most commits that modifies `pom.xml` may not include all the necessary code changes, e.g. "cleanup pom" commits.
Therefore, it's questionable to find code changes from the `pom.xml` modifying commit only.
2. Many projects have extremely large commits that is very hard to determine what has been done from this commit.
3. Some migrations only concerns configuration file changes, e.g. many logging migrations

See `rq1_depseq.ipynb` and `rq1_ground_truth.ipynb`.

## RQ2: For each metric, how effective it is in identifying these real migrations?

This step will be similar to the original RQ1.
See `rq2_metrics.ipynb`.

## RQ3: How good is our algorithm in discovering these migration rules?

This step will be similar to the original RQ2. We should add some more metrics though.
See `rq3_ranking.ipynb`.

## RQ4: How many additional migration rules can we discover, compared with [1]?

We also systematically extend the rules from the ground truth rules using our algorithm and compare with [1] to see how many new rules we can discover.

## References

1. Teyton, Cédric, et al. "A study of library migrations in java." Journal of Software: Evolution and Process 26.11 (2014): 1030-1052.
