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
Confidential computation

The objective of confidential computing is to evaluate a function on private datasets of two or more parties, while also ensuring that no party learns about anyone else's dataset, beyond what is revealed by the output of the computation. This problem is broadly referred to as secure function evaluation (SFE) and the two primary ways to handle it are Homomorphic Encryption (HE) and  Secure MultiParty Computation (MPC).

Homomorphic Encryption (HE) is an encryption scheme that allows computation over encrypted data directly, without the explicit requirement to decrypt it before performing the computation. Fully homomorphic encryption can be used to evaluate any arbitrary function of any depth [1]. A Secure MultiParty Computation (MPC) protocol ensures that no participant in the protocol learns more than what they could have learned in the presence of a trusted third-party to perform the computation [2]. Yao's Garbled Circuit (GC) [3] is one of the first protocols that allowed MPC and forms the foundation of many different MPC protocols today. It allows two parties to securely compute a function that has been converted into a boolean circuit. Although theoretical results about the existence of secure MPC protocols for performing any distributed computational task were provided long ago [4,5], it is still a challenging problem to design protocols that have practical communication, memory, and computational costs.

In the case of machine learning, designing model architectures that are generic enough to perform well over existing datasets, while at the same time reducing the computational cost of SFE is a tricky task. Two primary approaches to tackle this are: 1) Replacing some operations which are computationally expensive in an SFE environment with their approximations, which are more efficient; or devising alternative computation strategies which perform the same operations but utilize sub-operations that are more efficient in the SFE setting. 2) Reducing data processing precision by quantizing values instead of processing them in full-precision, thereby reducing the computation cost [6]. However, both approaches may come at a cost of accuracy due to the approximations used for reducing computational load for SFE. We work towards understanding the trade-off and an optimal compromise between the accuracy of a model architecture and its computational performance in an SFE setting.


### References

[1] C. Gentry. A fully homomorphic encryption scheme. AAI3382729, Advisor: D. Boneh, PhD thesis, Stanford, CA, USA, 2009.

[2]  D. Evans, V. Kolesnikov, and M. Rosulek. A pragmatic introduction to secure multi-party computation. Foundations and Trends in Privacy and Security, vol. 2, no. 2-3, pp. 70–246, 2018.

[3] A. C. Yao. How to generate and exchange secrets. In 27th Annual Symposium on Foundations of Computer Science (SCFS 1986), Oct. 1986, pp. 162–167.

[4] O. Goldreich, S. Micali and A. Wigderson. How to Play any Mental Game { A Completeness Theorem for Protocols with Honest Majority. In the 19th STOC, pages 218{229, 1987. Details in Foundations of Cryptography: Volume 2 { Basic Applications (Cambridge University Press 2004), by Oded Goldreich.

[5] R. Canetti, Y. Lindell, R. Ostrovsky and A. Sahai. Universally Composable Two-Party and Multi-Party Computation. In the 34th STOC, pages 494{503, 2002. Full version available at http://eprint.iacr.org/2002/140.

[6] M. S. Riazi, M. Samragh, H. Chen, K. Laine, K. Lauter, and F. Koushanfar. XONN: XNOR-based Oblivious Deep Neural Network Inference. In 28th USENIX Security Symposium (USENIX Security 19)

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
