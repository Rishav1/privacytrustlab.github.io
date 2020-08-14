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
## Data privacy and confidentiality

When we perform any computation on a sensitive dataset (e.g. calculating statistics or training machine learning models), it is important to understand the privacy risk of that computation to the individuals in the dataset. An obvious direct privacy risk is the exposure of sensitive data *during* the computation. A more subtle privacy threat is the indirect leakage about data *through the output* of computation. The former is generally referred to as protecting confidentiality in computation and the latter as privacy preserving computation.

## Privacy preserving computation

Releasing even simple statistics (such as averages) a dataset can reveal the information about the specific records in the dataset, for example, presence of an individual in the dataset [1]. Given enough such seemingly simple statistics, it is possible to reconstruct the entire dataset with high probability [2]. Hence, we should seek to have mathematically rigorous privacy guarantees, when releasing computations on sensitive datasets. The level of privacy protection and success of an attacker are essentially different narratives of the same story. Hence the two important angles to approach this problem are: Developing techniques to automatically protect privacy of the input data and designing attack algorithms to measure information leakage from the output of a computation.

Differential privacy is a widely accepted notion of statistical privacy, which requires the output of computation to be more or less unchanged when a single record in the dataset is modified [3]. This is generally achieved by randomizing the output of the computation through addition of noise [4]. Handling the trade-offs between accuracy and privacy-risks is a key challenge when using differential privacy for practical applications and modern, complex machine learning methods [5,6]. It is important to design mechanisms that can provide better accuracy for a given privacy guarantee and are easy to use in practical settings.

Membership inference attacks, where an attacker infers if a particular record was present in the training dataset just by observing the outputs of a computation, are considered as a measure of the information leakage about the input data from the output. These attacks were tested on Machine Learning as a Service platforms offered by Google and Amazon, and also in federated learning settings, showing the privacy vulnerability of such systems [7,8,9]. Our tool ML Privacy Meter [10] quantifies the privacy risk of machine learning models by simulating attackers that perform Membership Inference attacks, assuming different levels of background knowledge and capabilities of the attacker, and is based on state-of-the-art attack techniques.

A different approach to protecting the privacy of individuals in a sensitive dataset, while also releasing outputs of computations on it is generating synthetic data from the original dataset and using the synthetic to perform computations [11,12]. The key challenges in generating synthetic data are achieving scalability (across dimensions of the data) and guaranteeing a decent privacy-accuracy trade-off for a given task. Also, utility of the generated synthetic data heavily depends on the target dataset. Theoretically there cannot exist a generic synthetic dataset that can accurately answer queries beyond a certain limit. Hence, the key challenge here is designing techniques that are practical, scalable, and achieve decent utility levels for a given task, at various levels of privacy guarantees.

### References

[1] Homer, Nils, et al. "Resolving individuals contributing trace amounts of DNA to highly complex mixtures using high-density SNP genotyping microarrays." PLoS Genet 4.8 (2008): e1000167.

[2] Dinur, Irit, and Kobbi Nissim. "Revealing information while preserving privacy." Proceedings of the twenty-second ACM SIGMOD-SIGACT-SIGART symposium on Principles of database systems. 2003.

[3] Dwork, Cynthia, et al. "Calibrating noise to sensitivity in private data analysis." Theory of cryptography conference. Springer, Berlin, Heidelberg, 2006.

[4] Dwork, Cynthia, and Aaron Roth. "The algorithmic foundations of differential privacy." Foundations and Trends in Theoretical Computer Science 9.3-4 (2014): 211-407.

[5] Shokri, Reza, and Vitaly Shmatikov. "Privacy-preserving deep learning." Proceedings of the 22nd ACM SIGSAC conference on computer and communications security. 2015.

[6] Abadi, Martin, et al. "Deep learning with differential privacy." Proceedings of the 2016 ACM SIGSAC Conference on Computer and Communications Security. 2016.

[7] Shokri, Reza, et al. "Membership inference attacks against machine learning models." 2017 IEEE Symposium on Security and Privacy (SP). IEEE, 2017.

[8] Nasr, Milad, Reza Shokri, and Amir Houmansadr. "Comprehensive privacy analysis of deep learning: Passive and active white-box inference attacks against centralized and federated learning." 2019 IEEE Symposium on Security and Privacy (SP). IEEE, 2019.

[9] Melis, Luca, et al. "Exploiting unintended feature leakage in collaborative learning." 2019 IEEE Symposium on Security and Privacy (SP). IEEE, 2019.

[10] Murakonda, Sasi Kumar, and Reza Shokri. "ML Privacy Meter: Aiding Regulatory Compliance by Quantifying the Privacy Risks of Machine Learning." arXiv preprint arXiv:2007.09339 (2020).

[11] Zhang, Jun, et al. "Privbayes: Private data release via bayesian networks." ACM Transactions on Database Systems (TODS) 42.4 (2017): 1-41.

[12] Vincent Bindschaedler, Reza Shokri, and Carl A Gunter. 2017. Plausible deniability for privacy-preserving data synthesis. Proceedings of the VLDB Endowment 10, 5(2017), 481–492.
