---
layout: post
mathjax: true
title:  "Data Privacy"
date:   2020-08-06 11:00:00 +0700
categories: jekyll update
tags: main
author: <a href='https://www.linkedin.com/in/sasi-kumar-murakonda/'>Sasi Kumar Murakonda</a> and <a href='https://www.comp.nus.edu.sg/~mstrobel/'>Martin Strobel</a>
excerpt: ...
---
## Data privacy - Privacy preserving computation

When releasing statistics about a sensitive dataset, we want to ensure that the privacy of individuals in the dataset is not violated. Releasing even simple statistics (such as averages) about a dataset can reveal information about the specific records in the dataset, for example, presence of an individual in the dataset [1]. Given enough such seemingly simple statistics, it is possible to reconstruct the entire dataset with high probability [2]. As an individual, one would want the output of a computation to not change by much due to their inclusion in the dataset. A power to plausibly deny the presence or absence in the dataset is wished for. Hence, mathematically rigorous privacy guarantees to individuals are required when releasing computations on sensitive datasets. The level of privacy protection and success of an attacker are essentially different narratives of the same story. Hence the two important angles to approach this problem are: Designing attack algorithms to measure information leakage from the output of a computation and developing techniques to automatically protect privacy of the input data.

Tracing attacks, also known as membership inference attacks, where an attacker infers if a particular record was present in the training dataset just by observing the outputs of a computation, are considered as a measure of the information leakage about the input dataset from the output. Tracing attacks have been extensively studied for summary statistics, where independent statistics (e.g., mean) of attributes of high-dimensional data are released [1,3,4]. Theoretical frameworks are available to analyze the upper bound on the power of these inference attacks [3], and their robustness to noisy statistics [4], but only for simple models such as product distributions. These attacks were also tested against machine learning models on ML as a Service platforms offered by Google and Amazon, and also in federated learning settings, showing the privacy vulnerability of such systems [5,6,7]. Whether it is simple statistics or complex ML models, learning refers to estimation of parameters. The difference between these estimated values of a parameter (from the dataset) and true value (estimated from the entire population) is what poses the privacy threat to the dataset. The key to protecting privacy of an individual record in the dataset is ensuring that its affect on this estimation of parameters is minimal.

Differential privacy is a widely accepted notion of statistical privacy, which requires the output of computation to be more or less unchanged when a single record in the dataset is modified [8]. This is generally achieved by randomizing the output of the computation through addition of noise [9]. If a trusted curator that can collect all the records exists, then the noise can be added at the output of the computation, while providing privacy guarantees to each individual record. This is generally referred as global differential privacy. If no such trusted curator exists, each individual record/contribution needs to be randomized while estimating the final output over all records. This stronger requirement is referred as local differential privacy. Although, it is always desirable to satisfy the stronger notion of local differential privacy, the utility that can be achieved under the notion of global differential privacy is far greater compared to that of local differential privacy. 

When using differential privacy for practical applications, handling the trade-off between accuracy of the outputs and privacy-risk to individual data is a key challenge. Multiple relaxations of the standard definition of differential privacy were proposed to improve the utility and achieve better bounds for composition of privacy loss over multiple computations [10, 11, 12]. Among these, Renyi Differential Privacy (RDP) [13] is widely used because of its ability to provide tight bounds on composition of tail privacy losses. RDP is based on the notion of Renyi Divergence between the distributions of output computed over two neighboring datasets, as opposed to the notion of Max Divergence used in standard differential privacy. Contrast to the divergence based relaxations of differential privacy, the recently introduced notion of "f-differential privacy" is based on a hypothesis testing framework [14]. Privacy guarantees are defined in terms of the trade-off between Type-I and Type-II errors that are achievable by the most powerful and informed attacker, when inferring about the presence of any individual in the dataset. It is worth emphasizing that these are all just desired definitions of a privacy guarantee and the computation of output should be appropriately randomized to satisfy these definition of privacy.

A different approach to protecting the privacy of individuals in a sensitive dataset, while also releasing outputs of computations on it is generating synthetic data from the original dataset and using the synthetic to perform computations [15,16]. The key challenges in generating synthetic data are achieving scalability (across dimensions of the data) and guaranteeing a decent privacy-accuracy trade-off for a given task. Also, utility of the generated synthetic data heavily depends on the target dataset and the specific task by which utility is measured. Theoretically there cannot exist a generic synthetic dataset that can accurately answer all queries beyond a certain limit, while also protecting privacy. Hence, the key challenge here is designing techniques that are practical, scalable, and achieve decent utility levels for a given task, at various levels of privacy guarantees.

### References

[1] Homer, Nils, et al. "Resolving individuals contributing trace amounts of DNA to highly complex mixtures using high-density SNP genotyping microarrays." PLoS Genet 4.8 (2008): e1000167.

[2] Dinur, Irit, and Kobbi Nissim. "Revealing information while preserving privacy." Proceedings of the twenty-second ACM SIGMOD-SIGACT-SIGART symposium on Principles of database systems. 2003.

[3] Sankararaman, Sriram, Guillaume Obozinski, Michael I. Jordan, and Eran Halperin. "Genomic privacy and limits of individual detection in a pool." Nature genetics 41, no. 9 (2009): 965-967.

[4] Dwork, Cynthia, Adam Smith, Thomas Steinke, Jonathan Ullman, and Salil Vadhan. "Robust traceability from trace amounts." In 2015 IEEE 56th Annual Symposium on Foundations of Computer Science, pp. 650-669. IEEE, 2015.

[5] Shokri, Reza, et al. "Membership inference attacks against machine learning models." 2017 IEEE Symposium on Security and Privacy (SP). IEEE, 2017.

[6] Nasr, Milad, Reza Shokri, and Amir Houmansadr. "Comprehensive privacy analysis of deep learning: Passive and active white-box inference attacks against centralized and federated learning." 2019 IEEE Symposium on Security and Privacy (SP). IEEE, 2019.

[7] Melis, Luca, et al. "Exploiting unintended feature leakage in collaborative learning." 2019 IEEE Symposium on Security and Privacy (SP). IEEE, 2019.

[8] Dwork, Cynthia, et al. "Calibrating noise to sensitivity in private data analysis." Theory of cryptography conference. Springer, Berlin, Heidelberg, 2006.

[9] Dwork, Cynthia, and Aaron Roth. "The algorithmic foundations of differential privacy." Foundations and Trends in Theoretical Computer Science 9.3-4 (2014): 211-407.

[10] C. Dwork, K. Kenthapadi, F. McSherry, I. Mironov, and M. Naor, “Our data, ourselves: Privacy via distributed noise generation,” inAdvancesin Cryptography—Eurocrypt ’06.  Springer, 2006, pp. 486–503.

[11] Dwork, Cynthia, and Guy N. Rothblum. "Concentrated differential privacy." arXiv preprint arXiv:1603.01887 (2016).

[12] Bun, Mark, and Thomas Steinke. "Concentrated differential privacy: Simplifications, extensions, and lower bounds." In Theory of Cryptography Conference, pp. 635-658. Springer, Berlin, Heidelberg, 2016.

[13] Mironov, Ilya. "Rényi differential privacy." In 2017 IEEE 30th Computer Security Foundations Symposium (CSF), pp. 263-275. IEEE, 2017.

[14] Dong, Jinshuo, Aaron Roth, and Weijie J. Su. "Gaussian differential privacy." arXiv preprint arXiv:1905.02383 (2019).

[15] Zhang, Jun, et al. "Privbayes: Private data release via bayesian networks." ACM Transactions on Database Systems (TODS) 42.4 (2017): 1-41.

[16] Vincent Bindschaedler, Reza Shokri, and Carl A Gunter. 2017. Plausible deniability for privacy-preserving data synthesis. Proceedings of the VLDB Endowment 10, 5(2017), 481–492.
