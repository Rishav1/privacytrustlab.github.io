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
The key to building trustworthy AI systems is ensuring that they are robust, fair, interpretable, and can maintain the privacy and confidentiality of sensitive data. AI governance frameworks emphasize on these aspects of AI in the assessment lists for ethical and trustworthy AI. Our research focuses on understanding the trade-offs between different requirements for trust in practical machine learning systems. And especially on quantifying the privacy threats to sensitive data in data processing systems.

## Data privacy and confidentiality

When building machine learning models using sensitive data, organizations should ensure that the data processed is adequately protected. For projects involving machine learning on personal data, it is mandatory from Article 35 of the GDPR to perform a Data Protection Impact Assessment (DPIA). The obvious privacy risk lies in is the exposure of sensitive data *during* the computation. Can we trust the party that is running the computation on sensitive data? A more subtle privacy threat is the indirect leakage about data *through the output* of computation. Can we trust the party that is observing the output of the computation? The former is generally referred to as protecting confidentiality in computation and the latter as privacy-preserving computation.

### Confidential computation

The objective of confidential computing is to evaluate a function on a private dataset, without exposing the input data beyond what is revealed by the output of the computation. This problem is broadly referred to as secure function evaluation (SFE) and depending on the setting, there are two primary ways to handle it. If a single participant wants to outsource computation on sensitive data, without revealing the data, then Homomorphic Encryption (HE) can be used. Whereas, if there are multiple participants and a function needs to be jointly evaluated, then Secure MultiParty Computation (MPC) can be used to evaluate the function, while also ensuring that no party learns about anyone else's dataset.

Homomorphic Encryption (HE) is an encryption scheme that allows computation over encrypted data directly, without the explicit requirement to decrypt it before performing the computation. Fully homomorphic encryption can be used to evaluate any arbitrary function of any depth [[Gentry, 2008](#Gentry09)]. A Secure MultiParty Computation (MPC) protocol ensures that no participant in the protocol learns more than what they could have learned in the presence of a trusted third-party to perform the computation [[Evans et al., 2018](#Evans18)]. [[Yao, 1986](#Yao86)]'s Garbled Circuit (GC) is one of the first protocols that allowed MPC and forms the foundation of many different MPC protocols today. It allows two parties to securely compute a function that has been converted into a boolean circuit. Although theoretical results about the existence of secure MPC protocols for performing any distributed computational task were provided long ago [[Goldreich et al., 2004](#Goldreich04); [Canetti et al., 2018](#Canetti02)], it is still a challenging problem to design protocols that have practical communication, memory, and computational costs.

In the case of machine learning, designing model architectures that are generic enough to perform well over existing datasets, while at the same time reducing the computational cost of SFE is a tricky task. Current work focuses on understanding the trade-off between the accuracy a model architecture can achieve and its computational performance in an SFE setting. This involves designing techniques such as (but not limited to) replacing computationally expensive operations in SFE with their efficient approximations, devising alternative computation strategies that perform the same operations but utilize sub-operations that are more efficient, and reducing data processing precision by quantizing values.

### References

<a name="Gentry09"></a> C. Gentry. A fully homomorphic encryption scheme. AAI3382729, Advisor: D. Boneh, PhD thesis, Stanford, CA, USA, 2009.

<a name="Evans18"></a> D. Evans, V. Kolesnikov, and M. Rosulek. A pragmatic introduction to secure multi-party computation. Foundations and Trends in Privacy and Security, vol. 2, no. 2-3, pp. 70–246, 2018.

<a name="Yao86"></a> A. C. Yao. How to generate and exchange secrets. In 27th Annual Symposium on Foundations of Computer Science (SCFS 1986), Oct. 1986, pp. 162–167.

<a name="Goldreich04"></a> O. Goldreich, S. Micali and A. Wigderson. How to Play any Mental Game { A Completeness Theorem for Protocols with Honest Majority. In the 19th STOC, pages 218{229, 1987. Details in Foundations of Cryptography: Volume 2 { Basic Applications (Cambridge University Press 2004), by Oded Goldreich.

<a name="Canetti02"></a> R. Canetti, Y. Lindell, R. Ostrovsky and A. Sahai. Universally Composable Two-Party and Multi-Party Computation. In the 34th STOC, pages 494{503, 2002. Full version available at http://eprint.iacr.org/2002/140.

## Data Privacy in Machine Learning

When we perform any computation on a sensitive dataset (e.g. calculating statistics or training machine learning models), it is important to understand the privacy risk of that computation to the individuals in the dataset. Machine learning models pose a subtle privacy risk to the data by indirectly revealing information through the model's predictions and parameters. Guidances released by the Information Commissioner’s Office (UK) and the National Institute of Standards and Technology (US) emphasize on the threats to data from models and recommend organizations to account for and estimate these risks to comply with data protection regulations.

How do we define and quantify this privacy risk from machine learning models to the individuals whose data is used to train the model? To do so, we need to distinguish two types of information that we can learn from the model: 1) Information that is generic to members of the population and 2) Information that is specific to members in the training set. Note that members of the training set are also members of the population. Any information that the model reveals about particular members in the training data beyond what it reveals about an arbitrary member of the population is a privacy threat to the training data. A key focus of research is designing inference attacks to quantify this privacy loss.

Inference attacks on machine learning algorithms fall into two fundamental and related categories: tracing (a.k.a. membership inference) attacks, and reconstruction attacks [[Dwork et al., 2017](#Dwork17)].  In a  reconstruction attack, the attacker’s objective is to infer attributes of the records in the training set [[Dinur et al., 2003](#Dinur03); [Wang et al., 2009](#Wang09)]. In a membership inference attack, however, the attacker’s objective is to infer if a particular individual data record was included in the training dataset [[Homer et al., 2008](#Homer08); [Sankararaman et al., 2009](#Sankararaman09); [Dwork et al., 2015](#Dwork15); [Shokri et al., 2017](#Shokri17); [Nasr19 et al., 2019](#Nasr19)]. Accuracy of inference attacks can be used as metrics to quantify leakage from machine learning models, and also to measure the effectiveness of privacy protection techniques.
 
Differential Privacy [[Dwork et al., 2006](#Dwork06)] is a notion of privacy, wherein the outputs of a computation should be indistinguishable when any single record in the data is modified. If the training process is differentially private [[Shokri et al., 2015](#Shokri15); [Abadi et al., 2016](#Abadi16)], the probability of producing a given model from a training dataset that includes a particular record is close to the probability of producing the same model when this record is not included. Differentially private models are, by construction, secure against inference attacks. One obstacle is that differentially private models may significantly reduce the model’s prediction depending on how strict of a privacy guarantee is required.

Our tool ML Privacy Meter quantifies the privacy risk to data from models through state-of-the-art membership inference attack techniques. By providing practical estimates of the privacy risk posed for different settings, the meter can help in selecting appropriate parameters for the necessary differential privacy guarantee. The tool can also guide practitioners in regulatory compliance by helping them analyze, identify, and minimize the threats to data, when deploying machine learning models [[Murakonda et al., 2020](#Murakonda20)].

### References

<a name="Dwork17"></a> Dwork, Cynthia, Adam Smith, Thomas Steinke, and Jonathan Ullman. "Exposed! a survey of attacks on private data." (2017).

<a name="Dinur03"></a> Dinur, Irit, and Kobbi Nissim. "Revealing information while preserving privacy." Proceedings of the twenty-second ACM SIGMOD-SIGACT-SIGART symposium on Principles of database systems. 2003.

<a name="Wang09"></a> Wang, Rui, Yong Fuga Li, XiaoFeng Wang, Haixu Tang, and Xiaoyong Zhou. "Learning your identity and disease from research papers: information leaks in genome wide association study." In Proceedings of the 16th ACM conference on Computer and communications security, pp. 534-544. 2009.

<a name="Homer08"></a> Homer, Nils, et al. "Resolving individuals contributing trace amounts of DNA to highly complex mixtures using high-density SNP genotyping microarrays." PLoS Genet 4.8 (2008): e1000167.

<a name="Sankararaman09"></a> Sankararaman, Sriram, Guillaume Obozinski, Michael I. Jordan, and Eran Halperin. "Genomic privacy and limits of individual detection in a pool." Nature genetics 41, no. 9 (2009): 965-967.

<a name="Dwork15"></a> Dwork, Cynthia, Adam Smith, Thomas Steinke, Jonathan Ullman, and Salil Vadhan. "Robust traceability from trace amounts." In 2015 IEEE 56th Annual Symposium on Foundations of Computer Science, pp. 650-669. IEEE, 2015.

<a name="Shokri17"></a> Shokri, Reza, et al. "Membership inference attacks against machine learning models." 2017 IEEE Symposium on Security and Privacy (SP). IEEE, 2017.

<a name="Nasr19"></a> Nasr, Milad, Reza Shokri, and Amir Houmansadr. "Comprehensive privacy analysis of deep learning: Passive and active white-box inference attacks against centralized and federated learning." 2019 IEEE Symposium on Security and Privacy (SP). IEEE, 2019.

<a name="Dwork06"></a> Dwork, Cynthia, et al. "Calibrating noise to sensitivity in private data analysis." Theory of cryptography conference. Springer, Berlin, Heidelberg, 2006.

<a name="Shokri15"></a> Shokri, Reza, and Vitaly Shmatikov. "Privacy-preserving deep learning." Proceedings of the 22nd ACM SIGSAC conference on computer and communications security. 2015.

<a name="Abadi16"></a> Abadi, Martin, et al. "Deep learning with differential privacy." Proceedings of the 2016 ACM SIGSAC Conference on Computer and Communications Security. 2016.

<a name="Murakonda20"></a> Murakonda, Sasi Kumar, and Reza Shokri. "ML Privacy Meter: Aiding Regulatory Compliance by Quantifying the Privacy Risks of Machine Learning." arXiv preprint arXiv:2007.09339 (2020).

## Robustness

Can we trust a machine learning model to make critical decisions that can affect individuals' socio-economic status? Are the predictions of these models reliable? The predictive behavior of machine learning models can be maliciously modified by slightly perturbing the distribution of training and/or test data. Models that achieve high accuracy on clean data can be made to learn significantly different decision boundaries with the injection of a small amount of poisoned data [[Steinhardt et al., 2017](#Steinhardt17)]. Test data can be slightly perturbed to create adversarial examples, which look similar to the original data, but can make the model misclassify them. Understanding these threats against the functionality of machine learning models and designing systems that are robust to these attacks is one of the core requirements for building trustworthy AI.

Attackers can corrupt the training data of a machine learning model to achieve different objectives. For example, the attacker might want to degrade the overall test accuracy of the model (called indiscriminate attacks) or increase the loss on specific data points or sub-populations (called targeted attacks). They might even seek to create a backdoor in the learning system to misclassify the backdoor instances as a target label specified by the attacker (called backdoor attacks). Machine learning models were shown to be easily susceptible to these attacks, making their usage in safety-critical systems hard. 

To defend against data poisoning attacks, techniques for achieving training time robustness seek to learn a model that minimizes the out-of-training error even if the training dataset is noisy (or poisoned by an attacker). Data sanitization, also known as outlier detection and anomaly detection, is a very common type of defense against data poisoning attacks [[Cretu et al., 2008](#Cretu08); [Chen et al., 2018](#Chen18); [Tran et al., 2018](#Tran18)]. Intuitively,  anomaly detectors can filter poisoning data, if the attacker injects points that are very different from the clean data into the training set. However, attackers can generate points that are very similar to the true data distribution but still successfully mislead the model [[Koh et al., 2017](#Koh18); [Steinhardt et al., 2017](#Steinhardt17)]. A testing-stage defense against backdoor attacks reverses the backdoor trigger from the victim model and then fixes the model through retraining or pruning [[Wang et al., 2019](#Wang19)]. These defenses do not provide any theoretical guarantees of robustness.

Adversarial training, a commonly used defense against adversarial examples, aims to ensure that the model produces the same prediction even if the points generated from actual test distribution are slightly modified. The high-level idea is to generate a lot of adversarial examples and explicitly train the model not to be fooled by each of them. Most of the existing adversarial training based defenses do not provide robustness guarantees and demonstrate the robustness property via empirical results. However, adversarial robustness is difficult to measure and many existing defenses have been completely circumvented [[Athalye et al., 2018](#Athalye18)].

Designing provably robust algorithms for training in presence of poisoning data and for mitigating adversarial examples are still open problems.

### References

<a name="Steinhardt17"></a> Steinhardt, Jacob, Pang Wei W. Koh, and Percy S. Liang. "Certified defenses for data poisoning attacks." Advances in neural information processing systems. 2017.

<a name="Cretu08"></a> Cretu, Gabriela F., et al. "Casting out demons: Sanitizing training data for anomaly sensors." 2008 IEEE Symposium on Security and Privacy (sp 2008). IEEE, 2008.

<a name="Koh18"></a> Koh, Pang Wei, Jacob Steinhardt, and Percy Liang. "Stronger data poisoning attacks break data sanitization defenses." arXiv preprint arXiv:1811.00741 (2018).

<a name="Chen18"></a> Chen, Bryant, et al. "Detecting backdoor attacks on deep neural networks by activation clustering." arXiv preprint arXiv:1811.03728 (2018).

<a name="Tran18"></a> Tran, Brandon, Jerry Li, and Aleksander Madry. "Spectral signatures in backdoor attacks." Advances in Neural Information Processing Systems. 2018.

<a name="Wang19"></a> Wang, Bolun, et al. "Neural cleanse: Identifying and mitigating backdoor attacks in neural networks." 2019 IEEE Symposium on Security and Privacy (SP). IEEE, 2019.

<a name="Papernot17"></a> Papernot, Nicolas, et al. "Practical black-box attacks against machine learning." Proceedings of the 2017 ACM on Asia conference on computer and communications security. 2017.

<a name="Athalye18"></a> Athalye, Anish, Nicholas Carlini, and David Wagner. "Obfuscated gradients give a false sense of security: Circumventing defenses to adversarial examples." arXiv preprint arXiv:1802.00420 (2018).

## Fairness

On May 23rd 2016, the nonprofit news platform ProPublica published an article in which it accused an algorithm used to predict the recidivism risks of criminals in many parts of the U.S to be biased against black defendants [[Angwin et al., 2016](#Angwin16)]. The company behind rebutted the claim an argued that the analysis was faulty. The story nevertheless sparked new interest by the machine learning community into the problem of machine fairness and it is by far not the only example of uncovered bias of (machine learning) algorithms. Some famous examples are the work by [Buolamwini and Gebru, 2018](Buolamwini18) that demonstrated wide disparities in prediction accuracy, depending on skin type and gender, of commercial gender classification systems and the [news stories](https://www.reuters.com/article/us-amazon-com-jobs-automation-insight/amazon-scraps-secret-ai-recruiting-tool-that-showed-bias-against-women-idUSKCN1MK08G) about Amazon stopping the implementation of an AI recruitment tool after it showed bias against women. Problems with (un)fairness of machine learning algorithms can be found in many different application domains and solving them is an active area of research. 

Formally, training a machine learning model refers to finding a set of parameters that optimize an average loss function computed over training dataset. However, the parameters that minimize an overall loss might have very different outcomes for different subpopulations and it is often the majority, for which the most and the best data is available, that benefits most from these models. Before one can address unfairness a formal definition of what it means to be fair is needed. In fact many formal definitions with different focus have been proposed.  Some of these metrics focus on equality across sensitive groups [[Calders et al., 2009](#Calders09); [Hardt et al., 2016](#Hardt16)] others focus on the fairness of individuals and are trying to formalize a notion that similar people should be treated similarly [[Dwork et al., 2012](#Dwork12)]. Yet another approach is to establish causality [[Kusner et al., 2017](#Kusner17)] instead of relying on potentially biased correlations in the data. Many techniques are available to achieve group-based fairness such as pre-processing methods [[Madras et al., 2018](#Madras18); [Zemel et al., 2013](#Zemel13)], in-processing methods [[Agarwal et al., 2018](#Agarwal18); [Kamishima et al., 2011](#Kamishima11); [Zafar et al., 2015](#Zafar15); [Zafar et al., 2017](#Zafar17)], and post-processing methods [[Hardt et al., 2016](#Hardt16)].

Towards minimizing discrimination against a group, fair machine learning algorithms strive to equalize the behavior of a model across different groups, by imposing a fairness constraint on models. Imposing fairness constraints might come at a cost of the model’s performance and multiple definitions of fairness might not be even compatible with each other [[Corbett-Davies et al., 2018](#Corbett-Davies17); [Kleinberg et al., 2016](#Kleinberg16)]. It is shown that achieving equal calibration, false positive rate and false negative rate is impossible, if the fraction of positive labeled examples is different across sensitive groups.

In our recent work [[Hongyan et al., 2020](#Hongyan20)], we show that imposing group-fairness constraints on learning algorithms decreases their robustness to poisoning attacks. An attacker that can only control the sampling and labeling process for a fraction of the training data can significantly degrade the test accuracy of the models learned with fairness constraints. We also show that learning with fairness constraints in presence of such adversarial bias results in a classifier that not only has poor test accuracy but is also potentially more discriminatory on test data. In fact, from a practical perspective, such bias can easily and stealthily be perpetrated in many existing systems, as similar to historical discrimination and/or selection bias. While the goal of fairness is to promote the well-being of the protected groups, surprisingly being fair can cause harm in cases where an unconstrained objective would not when the data changes over time [Liu et al., 2019](#Liu19).

Hence, an important research direction in FairML is to study the accuracy, robustness guarantees of fair machine learning algorithms, the potential consequences of using such algorithms in presence of adversarially biased data, and how their behavior changes over time. Another interesting and crucial challenge is to design fair learning algorithms that are also robust to poisoning attacks.


### References 

<a name="Angwin16"></a> Angwin, Julia, et al. "Machine bias." ProPublica, May 23 (2016): 2016.

<a name="Buolamwini18"></a> Buolamwini, Joy, and Timnit Gebru. "Gender shades: Intersectional accuracy disparities in commercial gender classification." Conference on fairness, accountability and transparency. 2018.

<a name="Calders09"></a> Toon Calders, Faisal Kamiran, and Mykola Pechenizkiy. Building classifiers with independency constraints. In 2009 IEEE International Conference on Data Mining Workshops, pages 13–18. IEEE, 2009. 

<a name="Hardt16"></a>  Moritz Hardt, Eric Price, Nati Srebro, et al. Equality of opportunity in supervised learning. In Advances in neural information processing systems, pages 3315–3323, 2016.

<a name="Dwork12"></a> Cynthia Dwork, Moritz Hardt, Toniann Pitassi, Omer Reingold, and Richard Zemel. Fairness through awareness. In Innovations in Theoretical Computer Science (ITCS), pages 214–226, 2012.

<a name="Kusner17"></a> Matt J Kusner, Joshua Loftus, Chris Russell, and Ricardo Silva. Counterfactual fairness. In Advances in Neural Information Processing Systems, pages 4066–4076, 2017.

<a name="Madras18"></a> David Madras, Elliot Creager, Toniann Pitassi, and Richard Zemel. Learning adversarially fair and transferable representations. arXiv preprint arXiv:1802.06309, 2018.
		
<a name="Zemel13"></a> Rich Zemel, Yu Wu, Kevin Swersky, Toni Pitassi, and Cynthia Dwork. Learning fair representations. In International Conference on Machine Learning, pages 325–333, 2013.

<a name="Agarwal18"></a> lekh Agarwal, Alina Beygelzimer, Miroslav Dudik, John Langford, and Hanna Wallach. A reductions approach to fair classification. 2018.

<a name="Kamishima11"></a> Toshihiro Kamishima, Shotaro Akaho, and Jun Sakuma. Fairness-aware learning through regularization approach. In 2011 IEEE 11th International Conference on Data Mining Workshops, pages 643–650. IEEE, 2011.

<a name="Zafar15"></a> Muhammad Bilal Zafar, Isabel Valera, Manuel Gomez Rodriguez, and Krishna P Gummadi. Fairness constraints: Mechanisms for fair classification. arXiv preprint arXiv:1507.05259, 2015.

<a name="Zafar17"></a> Muhammad Bilal Zafar, Isabel Valera, Manuel Gomez Rodriguez, and Krishna P Gummadi. Fairness beyond disparate treatment & disparate impact: Learning classification without disparate mistreatment. In Proceedings of the 26th international conference on world wide web, pages 1171–1180, 2017.

<a name="Corbett-Davies17"></a> Sam Corbett-Davies, Emma Pierson, Avi Feller, Sharad Goel, and Aziz Huq. Algorithmic decision making and the cost of fairness. In Proceedings of the 23rd ACMSIGKDD International Conference on Knowledge Discovery and Data Mining, pages 797–806, 2017.

<a name="Kleinberg16"></a> Jon M. Kleinberg, Sendhil Mullainathan, and Manish Raghavan. Inherent trade-offs in the fair determination of risk scores. arXiv preprint arXiv:1609.05807, 2016.

<a name="Hongyan20"></a> Chang, Hongyan, et al. "On Adversarial Bias and the Robustness of Fair Machine Learning." arXiv preprint arXiv:2006.08669 (2020).

<a name="Liu19"></a> Liu, Lydia T., et al. "Delayed impact of fair machine learning." Proceedings of the 28th International Joint Conference on Artificial Intelligence. AAAI Press, 2019

</details>

## Interpretability
The inherent complexity of machine learning models makes it increasingly difficult to comprehend how and why they make certain classification decisions. Research on interpretable machine learning aims to counteract this development. At least three different significant motivations for interpretable machine learning have been identified [[Selbst et al., 2018](#Selbst18)]:

1) Explanations are necessary to give agency to individuals affected by an automated decision. Decisions based on incorrect data or wrong assumptions can only be corrected if they are understood. Finally, interpretability allows a person to strategically change their behavior and obtain a positive outcome in the future.

2) An auditor can use explanations to investigate a machine learning model. Explanations might help to improve a model's performance for its initial purpose. They can also be used to discover undesirable side-effects. 

3) From a moral point, it is inherently unjust to subject a human to black-box decision making. If a fully automatic algorithmic decision cannot be scrutinized, it automatically violates humans personhood, dignity, and autonomy. However, whether or not such a (human) right to explanation exists is still under discussion and has not been broadly legally established [[Wachter et al., 2017](#Wachter17)].

Techniques to achieve interpretable machine learning can be split into two subareas. The first area focuses on creating machine learning algorithms that are inherently interpretable. Those techniques mainly focus on restricting the type or size of used models. The second area focuses on creating explanations or interpretations for already existing methods. The latter can be divided into approaches focusing on a single decision and methods covering the entire model.

No clear and widely agreed-upon definition for interpretability exists. The terms explainability, transparency, and interpretability are often interchangeably used. The meaning of interpretability might vary widely from person to person as well among different application areas. Clear definitions will help to develop better-structured evaluation methods and metrics. Conducting comprehensive field tests to see which of the existing methods can obtain the desired results, will help to get from a collection of proposed tools to achieving the goals connected to the above motivations.

The exploration of interactions between interpretability and other aspects of machine learning like performance, privacy, fairness, and robustness will lead to new insights. While many authors claim that inherently interpretable models come with a significant tradeoff in a model's accuracy, others state that this tradeoff is minimal [[Rudin, 2019](#Rudin19)]. There is some evidence that model explanations leak private information about the model's data as well as the model itself [[Shokri, 2019](#Shokri19); [Milli, 2019](#Milli19)]. Finally, recent work questions the robustness of popular explanation methods [[Slack et al., 2029](#Slack20)].

### References

<a name="Selbst18"></a>  Selbst, Andrew D., and Solon Barocas. "The intuitive appeal of explainable machines." Fordham L. Rev. 87 (2018): 1085.

<a name="Wachter17"></a> Wachter, Sandra, Brent Mittelstadt, and Luciano Floridi. "Why a right to explanation of automated decision-making does not exist in the general data protection regulation." International Data Privacy Law 7.2 (2017): 76-99.

<a name="Rudin19"></a> Rudin, Cynthia. "Stop explaining black-box machine learning models for high stakes decisions and use interpretable models instead." Nature Machine Intelligence 1.5 (2019): 206-215.

<a name="Shokri19"></a> Shokri, Reza, Martin Strobel, and Yair Zick. "Privacy risks of explaining machine learning models." arXiv preprint arXiv:1907.00164 (2019).

<a name="Milli19"></a> Milli, Smitha, et al. "Model reconstruction from model explanations." Proceedings of the Conference on Fairness, Accountability, and Transparency. 2019.

<a name="Slack20"></a> Slack, Dylan, et al. "Fooling lime and shap: Adversarial attacks on post hoc explanation methods." Proceedings of the AAAI/ACM Conference on AI, Ethics, and Society. 2020.


