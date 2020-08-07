---
layout: post
mathjax: true
title:  "Federated Learning"
date:   2020-08-06 10:00:00 +0700
categories: jekyll update
tags: main
author: <a href='https://www.linkedin.com/in/sasi-kumar-murakonda/'>Sasi Kumar Murakonda</a> and <a href='https://www.comp.nus.edu.sg/~mstrobel/'>Martin Strobel</a>
excerpt: ...
---

How do we train accurate models on sensitive data from multiple participants, while reducing the concerns of data privacy and purpose limitation? In this context, federated learning (FL) has emerged as a promising paradigm to address this problem, which enables all participants to learn a highly accurate global model while ensuring that the data remains safe on local devices [1, 2]. Back to 2015, Shokri et al. [1] introduced  the concept of distributed deep learning and proposed Distributed Selective Stochastic Gradient Descent (DSSGD) protocol, and in 2016, Google further extended distributed deep learning to federated learning (FL), which shares the same spirit as the distributed learning [2]. In federated learning (distributed learning), each participant downloads the global model from the server and improves the model using their local private dataset. The only information that parties share with the server is the local (gradient) update. These individual updates are aggregated at the server to improve the global model. Hence, a highly accurate model can be learned from multiple private datasets while also ensuring that the data remains locally

Although FL is proposed as a privacy-preserving means that trains a model without sharing local private data, the privacy issue still exists. Recent works have manifested that exchanging model gradients is not sufficient to provide reasonable privacy guarantees [4,5,6]. The possible methods to reduce privacy leakage include sharing fewer gradients, dimensionality reduction of the input space, sharing less sensitive information as local updates [3]. Although these methods are commonly easy to implement and only incur a slight reduction in accuracy,  there is no provable guarantee for the privacy leakage. The widely-accepted and provable defense is differential privacy (DP) which guarantees that the output distributions are close for adjacent datasets. In the FL system, particularly, there are participant-level differential privacy (DP) [7], and record-level differential privacy (DP) [8]. However, the privacy guarantee comes at the cost of a significant accuracy drop of the learned model. In addition to differential privacy, secure multi-party computation can help guard against information leakage from the updates of a single user [9], however, the security comes at the cost of a large communication cost.

In an FL system, the (potentially malicious) participants can sabotage the collaborative learning process by manipulating local updates attacks. It has been manifested that existing FL algorithms are not robust to adversarial updates [10,11,12] and even a single party can severely disrupt the global model. There are a few defense mechanisms proposed in the literature [10,12,13] that replace the vulnerable aggregation algorithm in FL with more robust aggregation algorithms. However, as shown in [10], the high dimensionality of commonly used deep learning models explode the theoretical error of these robust aggregations and cannot provide robustness guarantee for the global model.

Most of the existing FL frameworks share gradients as local updates with the server, which is typically limited only to the homogeneous FL architectures. It is worthwhile to explore the question of whether it is essential to share the model's gradients. A more recent work revolutionizes FL by sharing less sensitive model predictions, which allows collaboration between models with heterogeneous architectures [3] and also circumvents security and privacy issues.

In conclusion, the privacy issue in FL system is still critical. The open research question is how to build FL systems that provide a provable privacy guarantee and also preserve the utility of the learned model. Designing a federated learning system that is robust to the malicious participants' updates and preserves the accuracy of the global model is still an open problem.

### References
[1] Shokri, Reza, and Vitaly Shmatikov. "Privacy-preserving deep learning." Proceedings of the 22nd ACM SIGSAC conference on computer and communications security. 2015.

[2] McMahan, Brendan, et al. "Communication-efficient learning of deep networks from decentralized data." Artificial Intelligence and Statistics. 2017.

[3] Chang, Hongyan, et al. "Cronus: Robust and Heterogeneous Collaborative Learning with Black-Box Knowledge Transfer." arXiv preprint arXiv:1912.11279 (2019).

[4] Nasr, Milad, Reza Shokri, and Amir Houmansadr. "Comprehensive privacy analysis of deep learning: Passive and active white-box inference attacks against centralized and federated learning." 2019 IEEE Symposium on Security and Privacy (SP). IEEE, 2019.

[5] Melis, Luca, et al. "Exploiting unintended feature leakage in collaborative learning." 2019 IEEE Symposium on Security and Privacy (SP). IEEE, 2019.

[6] Zhu, Ligeng, Zhijian Liu, and Song Han. "Deep leakage from gradients." Advances in Neural Information Processing Systems. 2019.

[7] Brendan, M. H., et al. "Learning differentially private recurrent language models." International conference on learning representations, Vancouver, BC, Canada. Vol. 30. 2018.

[8] Abadi, Martin, et al. "Deep learning with differential privacy." Proceedings of the 2016 ACM SIGSAC Conference on Computer and Communications Security. 2016.

[9] Bonawitz, Keith, et al. "Practical secure aggregation for privacy-preserving machine learning." Proceedings of the 2017 ACM SIGSAC Conference on Computer and Communications Security. 2017.

[10] Blanchard, Peva, Rachid Guerraoui, and Julien Stainer. "Machine learning with adversaries: Byzantine tolerant gradient descent." Advances in Neural Information Processing Systems. 2017.

[11] Baruch, Gilad, Moran Baruch, and Yoav Goldberg. "A little is enough: Circumventing defenses for distributed learning." Advances in Neural Information Processing Systems. 2019.

[12]  El Mahdi El Mhamdi, Rachid Guerraoui, and S ́ebastien Louis Alexandre Rouault.  The hidden vulnerability of distributed learning in byzantium.  In International  Conference  on  Machine  Learning, number CONF, 2018.

[13] Dong Yin, Yudong Chen, Ramchandran Kannan, and Peter Bartlett.  Byzantine-robust distributed learning:  Towards optimal statistical rates.  In International Conference on Machine Learning, pages 5650–5659, 2018.
