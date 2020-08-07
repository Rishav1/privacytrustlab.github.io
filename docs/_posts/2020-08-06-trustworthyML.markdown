---
layout: post
mathjax: true
title:  "Trustworthy machine learning"
date:   2020-08-06 10:00:00 +0700
categories: jekyll update
tags: main
author: <a href='https://www.linkedin.com/in/sasi-kumar-murakonda/'>Sasi Kumar Murakonda</a> and <a href='https://www.comp.nus.edu.sg/~mstrobel/'>Martin Strobel</a>
excerpt: ...
---
The key to building trustworthy AI systems is ensuring that they are robust, fair, interpretable, and can maintain the privacy and confidentiality of sensitive data. AI governance frameworks emphasize on these aspects of AI in the assessment lists for ethical and trustworthy AI. Our research focuses on  understanding the trade-offs between different requirements for trust in practical machine learning systems. Especially on quantifying the privacy threats to sensitive data in data processing systems.


## Fairness

Training a machine learning model refers to finding a set of parameters that optimize an average loss function computed over training dataset. Optimizing for such average prediction accuracy can come at the expense of fairness, as the performance of the model might not be optimal on certain sub-populations. As per the regulatory requirements, it is mandatory to ensure that decision making systems are not inherently biased against certain protected groups, that were historically discriminated against. 

In order to address this problem, multiple definitions of fairness were proposed in the literature. Examples include metric equality across sensitive groups [1, 2], individual fairness [3], causality [4], and many techniques to satisfy group-based fairness such as pre-processing methods [5, 6], in-processing methods [7, 8, 9, 10], and post-processing methods [11].

Towards minimizing discrimination against a group, fair machine learning algorithms strive to equalize the behavior of a model across different groups, by imposing a fairness constraint on models. Imposing fairness constraints might come at a cost of the model’s performance. The effect of fair classification on accuracy and the compatibility of various definitions with each other have been studied in recent work [12, 13]. It is shown that achieving equal calibration, false positive rate and false negative rate is impossible, if the fraction of positive labeled examples is different across sensitive groups.

In our recent work [14], we show that imposing group-fairness constraints on learning algorithms decreases their robustness to poisoning attacks. We specifically provide evidence that an attacker that can only control the sampling and labeling process for a fraction of the training data can significantly degrade the test accuracy of the models learned with fairness constraints. We also show that learning with fairness constraints in presence of such adversarial bias results in a classifier that not only has poor test accuracy but is also potentially more discriminatory on test data. In fact, from a practical perspective, such bias can easily and stealthily be perpetrated in many existing systems, as similar to historical discrimination and/or selection bias.
In a recent development, the research community has also started to pay attention to the temporal impact of fair models. While the goal of fairness is to promote the well-being of the protected groups, [15] shows that being fair can cause harm in cases where an unconstrained objective would not when the data changes over time.

Hence, an important research direction in FairML is to study the accuracy, robustness guarantees of fair machine learning algorithms, the potential consequences of using such algorithms in presence of adversarially biased data, and how their behavior changes over time.  Another interesting and crucial challenge is to design fair learning algorithms that are also robust to poisoning attacks.

### References

[1] Toon Calders, Faisal Kamiran, and Mykola Pechenizkiy. Building classifiers with independency constraints. In 2009 IEEE International Conference on Data Mining Workshops, pages 13–18. IEEE, 2009.

[2] Moritz Hardt, Eric Price, Nati Srebro, et al. Equality of opportunity in supervised learning. In Advances in neural information processing systems, pages 3315–3323, 2016.

[3] Cynthia Dwork, Moritz Hardt, Toniann Pitassi, Omer Reingold, and Richard Zemel. Fairness through awareness. In Innovations in Theoretical Computer Science (ITCS), pages 214–226, 2012.

[4] Matt J Kusner, Joshua Loftus, Chris Russell, and Ricardo Silva. Counterfactual fairness. In Advances in Neural Information Processing Systems, pages 4066–4076, 2017.

[5] David Madras, Elliot Creager, Toniann Pitassi, and Richard Zemel. Learning adversarially fair and transferable representations. arXiv preprint arXiv:1802.06309, 2018.

[6] Rich Zemel, Yu Wu, Kevin Swersky, Toni Pitassi, and Cynthia Dwork. Learning fair representations. In International Conference on Machine Learning, pages 325–333, 2013.

[7] Alekh Agarwal, Alina Beygelzimer, Miroslav Dudik, John Langford, and Hanna Wallach. A reductions approach to fair classification. 2018.

[8] Toshihiro Kamishima, Shotaro Akaho, and Jun Sakuma. Fairness-aware learning through regularization approach. In 2011 IEEE 11th International Conference on Data Mining Workshops, pages 643–650. IEEE, 2011.

[9] Muhammad Bilal Zafar, Isabel Valera, Manuel Gomez Rodriguez, and Krishna P Gummadi. Fairness constraints: Mechanisms for fair classification. arXiv preprint arXiv:1507.05259, 2015.

[10] Muhammad Bilal Zafar, Isabel Valera, Manuel Gomez Rodriguez, and Krishna P Gummadi. Fairness beyond disparate treatment & disparate impact: Learning classification without disparate mistreatment. In Proceedings of the 26th international conference on world wide web, pages 1171–1180, 2017.

[11] Moritz Hardt, Eric Price, Nati Srebro, et al. Equality of opportunity in supervised learning. In Advances in neural information processing systems, pages 3315–3323, 2016.

[12] Sam Corbett-Davies, Emma Pierson, Avi Feller, Sharad Goel, and Aziz Huq. Algorithmic decision making and the cost of fairness. In Proceedings of the 23rd ACMSIGKDD International Conference on Knowledge Discovery and Data Mining, pages 797–806, 2017.

[13] Jon M. Kleinberg, Sendhil Mullainathan, and " Manish Raghavan. Inherent trade-offs in the fair determination of risk scores.

[14] Chang, Hongyan, et al. "On Adversarial Bias and the Robustness of Fair Machine Learning." arXiv preprint arXiv:2006.08669 (2020).

[15] Liu, Lydia T., et al. "Delayed impact of fair machine learning." Proceedings of the 28th International Joint Conference on Artificial Intelligence. AAAI Press, 2019

## Interpretability
The inherent complexity of machine learning models makes it increasingly difficult to comprehend how and why they make certain classification decisions. Research on interpretable machine learning aims to counteract this development. At least three different significant motivations for interpretable machine learning have been identified [1]:

1) Explanations are necessary to give agency to individuals affected by an automated decision. Decisions based on incorrect data or wrong assumptions can only be corrected if they are understood. Finally, interpretability allows a person to strategically change their behavior and obtain a positive outcome in the future.

2) An auditor can use explanations to investigate a machine learning model. Explanations might help to improve a model's performance for its initial purpose. They can also be used to discover undesirable side-effects. 

3) From a moral point, it is inherently unjust to subject a human to black-box decision making. If a fully automatic algorithmic decision cannot be scrutinized, it automatically violates humans personhood, dignity, and autonomy. However, whether or not such a (human) right to explanation exists is still under discussion and has not been broadly legally established [2].

Techniques to achieve interpretable machine learning can be split into two subareas. The first area focuses on creating machine learning algorithms that are inherently interpretable. Those techniques mainly focus on restricting the type or size of used models. The second area focuses on creating explanations or interpretations for already existing methods. The latter can be divided into approaches focusing on a single decision and methods covering the entire model.

No clear and widely agreed-upon definition for interpretability exists. The terms explainability, transparency, and interpretability are often interchangeably used. The meaning of interpretability might vary widely from person to person as well among different application areas. Clear definitions will help to develop better-structured evaluation methods and metrics. Conducting comprehensive field tests to see which of the existing methods can obtain the desired results, will help to get from a collection of proposed tools to achieving the goals connected to the above motivations.

The exploration of interactions between interpretability and other aspects of machine learning like performance, privacy, fairness, and robustness will lead to new insights. While many authors claim that inherently interpretable models come with a significant tradeoff in a model's accuracy, others state that this tradeoff is minimal [3]. There is some evidence that model explanations leak private information about the model's data as well as the model itself [4, 5]. Finally, recent work questions the robustness of popular explanation methods [6].

### References

[1] Selbst, Andrew D., and Solon Barocas. "The intuitive appeal of explainable machines." Fordham L. Rev. 87 (2018): 1085.

[2] Wachter, Sandra, Brent Mittelstadt, and Luciano Floridi. "Why a right to explanation of automated decision-making does not exist in the general data protection regulation." International Data Privacy Law 7.2 (2017): 76-99.

[3] Rudin, Cynthia. "Stop explaining black-box machine learning models for high stakes decisions and use interpretable models instead." Nature Machine Intelligence 1.5 (2019): 206-215.

[4] Shokri, Reza, Martin Strobel, and Yair Zick. "Privacy risks of explaining machine learning models." arXiv preprint arXiv:1907.00164 (2019).

[5] Milli, Smitha, et al. "Model reconstruction from model explanations." Proceedings of the Conference on Fairness, Accountability, and Transparency. 2019.

[6] Slack, Dylan, et al. "Fooling lime and shap: Adversarial attacks on post hoc explanation methods." Proceedings of the AAAI/ACM Conference on AI, Ethics, and Society. 2020.

## Robustness
A common assumption in machine learning is that the training data and the test data follow the same distribution. However, this assumption can be violated in practice, say due to injection of poisoning data into the training set by an attacker or due to corruption of test data (adversarial examples). Understanding these threats against functionality of machine learning models and designing systems that are robust to the attacks is one of the core requirements for building trustworthy AI.

Machine learning systems are susceptible to data poisoning attacks. Models that achieve high accuracy on clean data can be made to learn significantly different decision boundaries with the injection of a small amount of poisoned data [1]. In indiscriminate attacks, the attacker's objective is to degrade the test accuracy of the model. In targeted attacks, the attacker seeks to impose the loss on specific test data points or small sub-populations. In backdoor attacks, the attacker aims at creating backdoor instances so that the victim learning system will be misled to classify the backdoor instances as a target label specified by the attacker. Instead of attacking the training process, in case of adversarial examples, the attacker modifies test inputs to look similar to clean test examples but can make the model mis-predict them.  

Training time robustness aims to learn a model that minimizes the out-of-training (??) error even if the training dataset is noisy (or poisoned by an attacker). Data sanitization, also known as outlier detection and anomaly detection is a very common type of defense [2,4,5]. In poisoning attacks, the attacker is by definition injecting something into the training dataset that is very different from what it should include. Hence, we can use anomaly detectors to filter out training points that look suspicious. However, it has been shown that poisoning attacks can bypass sanitization defenses [3]. The attacker can generate poisoning points that are very similar to the true data distribution but that still successfully mislead the model. In addition, data sanitization defense can also break down when sanitization rules are created based on the poisoning dataset [1]. Testing-stage defense against backdoor attacks reverses a backdoor trigger from the victim model, and then fixes the model through retraining or pruning [6]. However, these defenses do not provide any theoretical guarantees of robustness.

The objective of test time robustness is to ensure that the model produces the same prediction for points generated from the actual test distribution, even if the points are slightly modified. Adversarial training is commonly used as a defense method against adversarial examples. The high-level idea is to generate a lot of adversarial examples and explicitly train the model not to be fooled by each of them. Most of the existing adversarial training based defenses do not provide robustness guarantees and demonstrate the robustness property via empirical results. However, as shown in [8], adversarial robustness is difficult to measure and most papers get it wrong enough that the results are meaningless.

In conclusion, designing provably robust algorithms for training in presence of poisoning data and for mitigating adversarial examples are still open problems.


### References

[1] Steinhardt, Jacob, Pang Wei W. Koh, and Percy S. Liang. "Certified defenses for data poisoning attacks." Advances in neural information processing systems. 2017.

[2] Cretu, Gabriela F., et al. "Casting out demons: Sanitizing training data for anomaly sensors." 2008 IEEE Symposium on Security and Privacy (sp 2008). IEEE, 2008.

[3] Koh, Pang Wei, Jacob Steinhardt, and Percy Liang. "Stronger data poisoning attacks break data sanitization defenses." arXiv preprint arXiv:1811.00741 (2018).

[4] Chen, Bryant, et al. "Detecting backdoor attacks on deep neural networks by activation clustering." arXiv preprint arXiv:1811.03728 (2018).

[5] Tran, Brandon, Jerry Li, and Aleksander Madry. "Spectral signatures in backdoor attacks." Advances in Neural Information Processing Systems. 2018.

[6] Wang, Bolun, et al. "Neural cleanse: Identifying and mitigating backdoor attacks in neural networks." 2019 IEEE Symposium on Security and Privacy (SP). IEEE, 2019.

[7] Papernot, Nicolas, et al. "Practical black-box attacks against machine learning." Proceedings of the 2017 ACM on Asia conference on computer and communications security. 2017.

[8] Athalye, Anish, Nicholas Carlini, and David Wagner. "Obfuscated gradients give a false sense of security: Circumventing defenses to adversarial examples." arXiv preprint arXiv:1802.00420 (2018).
