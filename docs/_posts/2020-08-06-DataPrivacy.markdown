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

When releasing statistics about a sensitive dataset, we want to ensure that the privacy of individuals in the dataset is not violated. Releasing even simple statistics (such as averages) about a dataset can reveal information about the specific records in the dataset, for example, the presence of an individual in the dataset [[Homer, 2008](#Homer08)]. Given enough such seemingly simple statistics, it is possible to reconstruct the entire dataset with high probability [[Dinur et al., 2003](#Dinur03)]. People desire the power to plausibly deny their presence or absence in any given dataset. Hence, the output of a computation should not change by much due to the inclusion of a single individual.  Mathematically rigorous privacy guarantees to individuals are required when releasing computations on sensitive datasets. The level of privacy protection and the success of an attacker trying to obtain information are essentially different narratives of the same story. Therefore, the two important angles to approach this problem are: Developing techniques to automatically protect the privacy of the input data and designing attack algorithms to measure information leakage from the output of a computation.

Tracing attacks, also known as membership inference attacks, where an attacker infers if a particular record was present in the training dataset just by observing the outputs of a computation, are considered as a measure of the information leakage about the input dataset from the output. Tracing attacks have been extensively studied for summary statistics, where independent statistics (e.g., mean) of attributes of high-dimensional data are released [[Homer, 2008](#Homer08); [Sankararaman et al., 2009](#Sankararaman09); [Dwork et al., 2015](#Dwork15)]. Theoretical frameworks are available to analyze the upper bound on the power of these inference attacks [[Sankararaman et al., 2009](#Sankararaman09)], and their robustness to noisy statistics [[Dwork et al., 2015](#Dwork15)], but only for simple models such as product distributions. These attacks were also tested against machine learning models on ML as a service platforms offered by Google and Amazon, and also in federated learning settings, showing the privacy vulnerability of such systems [[Shokri et al., 2017](#Shokri17); [Nasr et al., 2019](#Nasr19); [Melis et al., 2019](#Melis19)]. In its core, learning refers to the estimation of parameters, whether it is the calculation of simple statistics or complex ML models. The difference between these estimated values of a parameter (from the dataset) and true value (estimated from the entire population) is what poses the privacy threat to the dataset. The key to protecting the privacy of an individual record in the dataset is ensuring that its effect on this estimation of parameters is minimal.

Differential privacy is a widely accepted notion of statistical privacy. This approach requires the output of computation to be more or less unchanged when a single record in the dataset is modified [[Dwork et al., 2006](#Dwork06)]. This is generally achieved by randomizing the output of the computation through the addition of noise [[Dwork et al., 2014](#Dwork14)]. If a trusted curator that can collect all the records exists, the noise can be added at the output of the computation, while providing privacy guarantees to each record. This is generally referred to as global differential privacy. If no such trusted curator exists, each record/contribution needs to be randomized while estimating the final output. This stronger requirement is referred to as local differential privacy. Although it is always desirable to satisfy the stronger notion of local differential privacy, the utility that can be achieved under the notion of global differential privacy is far greater. 

When using differential privacy for practical applications, handling the trade-off between the accuracy of the outputs and privacy-risk to individual data is a key challenge. Multiple relaxations of the standard definition of differential privacy were proposed to improve utility and achieve better bounds on privacy loss over the composition of multiple computations [[Dwork et al., 2006](#Dwork06a); [Dwork et al., 2016](#Dwork16); [Bun and Steinke, 2016](#Bun16)]. Among these, Renyi Differential Privacy (RDP) [[Mironov, 2017](#Mironov17)] is widely used because of its ability to provide tight bounds on the composition of tail privacy losses. RDP is based on the notion of Renyi Divergence between the distributions of output computed over two neighboring datasets, as opposed to the notion of Max Divergence used in standard differential privacy. In contrast to the divergence based relaxations of differential privacy, the recently introduced notion of "f-differential privacy" is based on a hypothesis-testing framework [[Dong et al., 2019](#Dong19)]. Privacy guarantees are defined in terms of the trade-off between Type-I and Type-II errors that are achievable by the most powerful and informed attacker when inferring about the presence of any individual in the dataset. It is worth emphasizing that these are all just desirable definitions of privacy guarantees. A computation needs to be appropriately randomized to satisfy these definitions.

A different approach to protecting the privacy of individuals in a sensitive dataset, while also releasing outputs of computations on it, is generating synthetic data from the original dataset and using the synthetic to perform computations [[Zhang et al., 2017](#Zhang17); [Bindschaedler et al., 2017](#Bindschaedler17)]. The key challenges in generating synthetic data are achieving scalability (across dimensions of the data) and guaranteeing a decent privacy-accuracy trade-off for a given task. Also, the utility of the generated synthetic data heavily depends on the target dataset and the specific task by which utility is measured. Theoretically, there cannot exist a generic synthetic dataset that can accurately answer all queries beyond a certain limit, while also protecting privacy. Hence, the key challenge here is designing techniques that are practical, scalable, and achieve decent utility levels for a given task, at various levels of privacy guarantees.

### References

<a name="Homer08"></a> Homer, Nils, et al. "Resolving individuals contributing trace amounts of DNA to highly complex mixtures using high-density SNP genotyping microarrays." PLoS Genet 4.8 (2008): e1000167.

<a name="Dinur03"></a> Dinur, Irit, and Kobbi Nissim. "Revealing information while preserving privacy." Proceedings of the twenty-second ACM SIGMOD-SIGACT-SIGART symposium on Principles of database systems. 2003.

<a name="Sankararaman09"></a> Sankararaman, Sriram, Guillaume Obozinski, Michael I. Jordan, and Eran Halperin. "Genomic privacy and limits of individual detection in a pool." Nature genetics 41, no. 9 (2009): 965-967.

<a name="Dwork15"></a> Dwork, Cynthia, Adam Smith, Thomas Steinke, Jonathan Ullman, and Salil Vadhan. "Robust traceability from trace amounts." In 2015 IEEE 56th Annual Symposium on Foundations of Computer Science, pp. 650-669. IEEE, 2015.

<a name="Shokri17"></a> Shokri, Reza, et al. "Membership inference attacks against machine learning models." 2017 IEEE Symposium on Security and Privacy (SP). IEEE, 2017.

<a name="Nasr19"></a> Nasr, Milad, Reza Shokri, and Amir Houmansadr. "Comprehensive privacy analysis of deep learning: Passive and active white-box inference attacks against centralized and federated learning." 2019 IEEE Symposium on Security and Privacy (SP). IEEE, 2019.

<a name="Melis19"></a> Melis, Luca, et al. "Exploiting unintended feature leakage in collaborative learning." 2019 IEEE Symposium on Security and Privacy (SP). IEEE, 2019.

<a name="Dwork06"></a> Dwork, Cynthia, et al. "Calibrating noise to sensitivity in private data analysis." Theory of cryptography conference. Springer, Berlin, Heidelberg, 2006.

<a name="Dwork14"></a> Dwork, Cynthia, and Aaron Roth. "The algorithmic foundations of differential privacy." Foundations and Trends in Theoretical Computer Science 9.3-4 (2014): 211-407.

<a name="Dwork06a"></a> C. Dwork, K. Kenthapadi, F. McSherry, I. Mironov, and M. Naor, “Our data, ourselves: Privacy via distributed noise generation,” inAdvancesin Cryptography—Eurocrypt ’06.  Springer, 2006, pp. 486–503.

<a name="Dwork16"></a> Dwork, Cynthia, and Guy N. Rothblum. "Concentrated differential privacy." arXiv preprint arXiv:1603.01887 (2016).

<a name="Bun16"></a> Bun, Mark, and Thomas Steinke. "Concentrated differential privacy: Simplifications, extensions, and lower bounds." In Theory of Cryptography Conference, pp. 635-658. Springer, Berlin, Heidelberg, 2016.

<a name="Mironov17"></a> Mironov, Ilya. "Rényi differential privacy." In 2017 IEEE 30th Computer Security Foundations Symposium (CSF), pp. 263-275. IEEE, 2017.

<a name="Dong19"></a> Dong, Jinshuo, Aaron Roth, and Weijie J. Su. "Gaussian differential privacy." arXiv preprint arXiv:1905.02383 (2019).

<a name="Zhang17"></a> Zhang, Jun, et al. "Privbayes: Private data release via bayesian networks." ACM Transactions on Database Systems (TODS) 42.4 (2017): 1-41.

<a name="Bindschaedler17"></a> Vincent Bindschaedler, Reza Shokri, and Carl A Gunter. 2017. Plausible deniability for privacy-preserving data synthesis. Proceedings of the VLDB Endowment 10, 5(2017), 481–492.
