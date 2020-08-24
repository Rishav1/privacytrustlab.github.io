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

How do we train accurate models on sensitive data provided by multiple participants, while reducing concerns of data privacy and purpose limitation? Federated learning (FL) has emerged as a promising paradigm to address this problem. It enables multiple participants to learn a highly accurate global model while ensuring that their data remains safe on local devices [[Shokri et al., 2015](#Shokri15); [McMahan et al., 2017](#McMahan17)]. In federated learning, each participant downloads a global model from the server and improves the model using their local private dataset. The only information that the participants share with the server is the local (gradient) update. These individual updates are aggregated at the server to improve the global model. Hence, a highly accurate model can be learned from multiple private datasets while also ensuring that the data remains with each participant.

FL is being celebrated as a success that permits learning while also protecting the local private data but just exchanging model gradients can still pose a significant threat to the local data [[Nasr et al., 2019](#Nasr19); [Melis et al., 2019](#Melis19); [Zhu et al., 2019](#Zhu19)]. Possible methods to reduce this privacy leakage include sharing fewer gradients, dimensionality reduction of the input space, sharing less sensitive information as local updates [[Chang et al., 2019](#Chang19)]. Although these methods are easy to implement and only incur a slight reduction of accuracy, there are no provable privacy guarantees or bounds on private information leakage. 

The widely-accepted and provable defense is differential privacy (DP), which guarantees that the output distributions are close for adjacent datasets. In the FL system, one can choose to provide participant-level differential privacy [[Brendan et al., 2018](#Brendan18)], or record-level differential privacy [[Abadi et al., 2016](#Abadi16)] guarantees. However, these privacy guarantees come at a significant cost to the accuracy of the learned model. In addition to differential privacy, secure multi-party computation can help to guard against information leakage from the updates of a single user [[Bonawitz et al., 2017](#Bonawitz17)], however, this security comes with a large communication cost.

In an FL system, potentially malicious participants can sabotage the collaborative learning process by manipulating local updates. Existing FL algorithms are not robust to adversarial updates. Even a single party can severely disrupt the global model [[Blanchard et al., 2017](#Blanchard17); [Baruch et al., 2019](#Baruch19); [Mhamdi et al., 2018](#Mhamdi18)]. Robust aggregation techniques [Blanchard et al., 2017](#Blanchard17); [Mhamdi et al., 2018](#Mhamdi18); [Yin et al., 2018](#Yin18)] can be used as a defense to mitigate this problem. Yet, the high-dimensional nature of modern deep learning models results in a rapid decline of the theoretical error guarantees provided by these aggregation techniques [Blanchard et al., 2017](#Blanchard17)].

Existing FL frameworks share gradients as local updates with the server, which limits all the participants to train on the same model architecture sent by the server. Also, the gradients are usually of high dimension (same as the size as the model), sometimes even having a million dimensions for large deep learning models. It is worthwhile to explore the question of whether it is essential to share the model's gradients to convey information about the local data. More recent work remodels FL by sharing less sensitive model predictions, which allows collaboration between models with heterogeneous architectures [[Chang et al., 2019](#Chang19)] and also circumvents security and privacy issues.

To summarize, existing FL systems face significant privacy and robustness issues. An open research question is how to build FL systems that provide provable privacy guarantees and are robust to the malicious participants, while also preserving the utility of the learned model. 

### References
<a name="Shokri15"></a> Shokri, Reza, and Vitaly Shmatikov. "Privacy-preserving deep learning." Proceedings of the 22nd ACM SIGSAC conference on computer and communications security. 2015.

<a name="McMahan17"></a> McMahan, Brendan, et al. "Communication-efficient learning of deep networks from decentralized data." Artificial Intelligence and Statistics. 2017.

<a name="Chang19"></a> Chang, Hongyan, et al. "Cronus: Robust and Heterogeneous Collaborative Learning with Black-Box Knowledge Transfer." arXiv preprint arXiv:1912.11279 (2019).

<a name="Nasr19"></a> Nasr, Milad, Reza Shokri, and Amir Houmansadr. "Comprehensive privacy analysis of deep learning: Passive and active white-box inference attacks against centralized and federated learning." 2019 IEEE Symposium on Security and Privacy (SP). IEEE, 2019.

<a name="Melis19"></a> Melis, Luca, et al. "Exploiting unintended feature leakage in collaborative learning." 2019 IEEE Symposium on Security and Privacy (SP). IEEE, 2019.

<a name="Zhu19"></a> Zhu, Ligeng, Zhijian Liu, and Song Han. "Deep leakage from gradients." Advances in Neural Information Processing Systems. 2019.

<a name="Brendan18"></a> Brendan, M. H., et al. "Learning differentially private recurrent language models." International conference on learning representations, Vancouver, BC, Canada. Vol. 30. 2018.

<a name="Abadi16"></a> Abadi, Martin, et al. "Deep learning with differential privacy." Proceedings of the 2016 ACM SIGSAC Conference on Computer and Communications Security. 2016.

<a name="Bonawitz17"></a> Bonawitz, Keith, et al. "Practical secure aggregation for privacy-preserving machine learning." Proceedings of the 2017 ACM SIGSAC Conference on Computer and Communications Security. 2017.

<a name="Blanchard17"></a> Blanchard, Peva, Rachid Guerraoui, and Julien Stainer. "Machine learning with adversaries: Byzantine tolerant gradient descent." Advances in Neural Information Processing Systems. 2017.

<a name="Baruch19"></a> Baruch, Gilad, Moran Baruch, and Yoav Goldberg. "A little is enough: Circumventing defenses for distributed learning." Advances in Neural Information Processing Systems. 2019.

<a name="Mhamdi18"></a>  El Mahdi El Mhamdi, Rachid Guerraoui, and S ́ebastien Louis Alexandre Rouault.  The hidden vulnerability of distributed learning in byzantium.  In International  Conference  on  Machine  Learning, number CONF, 2018.

<a name="Yin18"></a>[ Dong Yin, Yudong Chen, Ramchandran Kannan, and Peter Bartlett.  Byzantine-robust distributed learning:  Towards optimal statistical rates.  In International Conference on Machine Learning, pages 5650–5659, 2018.
