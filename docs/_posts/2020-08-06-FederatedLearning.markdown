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

How do we train accurate models on sensitive data from multiple participants, while also reducing the concerns of data privacy and purpose limitation? Federated learning (FL) has emerged as a promising paradigm to address this problem. It enables multiple participants to learn a highly accurate global model while ensuring that their data remains safe on local devices [1, 2]. In federated learning, each participant downloads a global model from the server and improves the model using their local private dataset. The only information that the participants share with the server is the local (gradient) update. These individual updates are aggregated at the server to improve the global model. Hence, a highly accurate model can be learned from multiple private datasets while also ensuring that the data remains locally.

FL is being celebrated as a success that permits learning while also protecting the local private data but just exchanging model gradients can still pose significant threat to the local data [4,5,6]. Possible methods to reduce this privacy leakage include sharing fewer gradients, dimensionality reduction of the input space, sharing less sensitive information as local updates [3]. Although these methods are easy to implement and only incur a slight reduction in accuracy, there are no provable privacy guarantees or bounds on private information leakage. 

The widely-accepted and provable defense is differential privacy (DP), which guarantees that the output distributions are close for adjacent datasets. In the FL system, one can choose to provide participant-level differential privacy (DP) [7], or record-level differential privacy (DP) [8] guarantees. However, these privacy guarantees come at a significant cost in accuracy of the learned model. In addition to differential privacy, secure multi-party computation can help guard against information leakage from the updates of a single user [9], however, this security comes with a large communication cost.

In an FL system, (potentially malicious) participants can sabotage the collaborative learning process by manipulating local updates. Existing FL algorithms are not robust to adversarial updates and even a single party can severely disrupt the global model [10,11,12]. Robust aggregation techniques [10,12,13] can be used as a defense to prevent this problem to an extent. But the high-dimensional nature of modern deep learning models result in an explosion of the theoretical error guarantees provided by these aggregation techniques [10].

Existing FL frameworks share gradients as local updates with the server, which limits all the participants to train on the same model architecture sent by the server. Also, the gradients are usually of high dimension (same as the size of the model), sometimes even having a million dimensions for large deep learning models. It is worthwhile to explore the question of whether it is essential to share the model's gradients to convey information about the local data. A more recent work revolutionizes FL by sharing less sensitive model predictions, which allows collaboration between models with heterogeneous architectures [3] and also circumvents security and privacy issues.

To summarize, existing FL systems face significant privacy and robustness issues. An open research question is how to build FL systems that provide provable privacy guarantees and are robust to the malicious participants, while also preserving the utility of the learned model. 

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
