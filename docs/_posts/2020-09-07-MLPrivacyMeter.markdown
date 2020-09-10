---
layout: post
mathjax: true
title:  "ML Privacy Meter"
date:   2020-09-07 10:00:00 +0700
categories: jekyll update
tags: main
author: <a href='https://www.linkedin.com/in/sasi-kumar-murakonda/'>Sasi Kumar Murakonda</a> and <a href='https://www.comp.nus.edu.sg/~mstrobel/'>Martin Strobel</a>
excerpt: ...
---

## ML Privacy Meter

When building machine learning models using sensitive data, organizations should ensure that the data processed in such systems is adequately protected. For a safe and secure use of machine learning models, it is important to have a quantitative assessment of the privacy risks of these models, and to make sure that they do not reveal sensitive personal information about their training data. Data Protection regulations, such as GDPR, and AI governance frameworks require personal data to be protected when used in AI systems, and that the users have control over their data and awareness about how it is being used. For projects involving machine learning on personal data, it is mandatory from [Article 35 of GDPR](https://gdpr-info.eu/art-35-gdpr/) to perform a Data Protection Impact Assessment (DPIA). Thus, proper mechanisms need to be in place to quantitatively evaluate and verify the privacy of individuals in every step of the data processing pipeline in AI systems. 

[ML Privacy Meter](https://github.com/privacytrustlab/ml_privacy_meter) is a Python library that enables quantifying the privacy risks of machine learning models. The tool provides privacy risk scores which help in identifying data records among the training data that are under high risk of being leaked through the model. 

{:refdef: style="text-align: center;"}
<img src="/assets/2020-09-07-MLPrivacyMeter/ml-privacy-meter.png" width="75%">
{:refdef}

Machine learning models encode information about the datasets on which they are trained. The encoded information is supposed to reflect the general patterns underlying the population data. However, it is commonly observed that these models memorize specific information about some members of their training data [[Song et al., 2017](#Song17)] or be tricked to do so [[Carlini et al., 2019](#Carlini19)]. Models with high generalization gap as well as the models with high capacity (such as deep neural networks) are more susceptible to memorizing data points from their training set. This is reflected in the predictions of the model, which exhibits a different behavior on training data versus test data, and in the model's parameters which store statistically correlated information about specific data points in their training set. This vulnerability of machine learning models was shown using *membership inference attacks*, where an attacker detects the presence of a particular record in the training dataset of a model, just by observing the model. Machine learning models were shown to be susceptible to these attacks in both the black-box [[Shokri et al., 2017](#Shokri17)] and white-box settings [[Nasr19 et al., 2019](#Nasr19)]. The privacy risks of machine learning models can be evaluated as the accuracy of such inference attacks against their training data.  

In the black-box setting such as machine learning as a service offered on cloud platforms by companies such as [Amazon](https://aws.amazon.com/machine-learning), [Microsoft](https://studio.azureml.net), and [Google](https://cloud.google.com/prediction), we can only observe predictions of the model. This setting models the scenario can be used to measure the privacy risks against legitimate users of a model who seek predictions on their queries. In the white-box setting, we can also observe the parameters of the model. This reflects the scenario where a model is outsourced to a potentially untrusted server or to the cloud, or is shared with an aggregator in the federated learning setting. **ML Privacy Meter** implements membership inference attacks in both the black-box and white-box settings. Ability to detect membership in the dataset using the released models is a measure of information leakage about the individuals in the dataset from the model.

Guidances released published by the [Information Commissioner’s Office (ICO) for auditing AI](https://ico.org.uk/media/about-the-ico/consultations/2617219/guidance-on-the-ai-auditing-framework-draft-for-consultation.pdf) and the [National Institute of Standards and Technology (NIST) for securing applications of Artificial Intelligence](https://nvlpubs.nist.gov/nistpubs/ir/2019/NIST.IR.8269-draft.pdf) emphasize on the threats to data from models and recommend organizations to account for and estimate these risks to comply with data protection regulations. And they specifically mention membership inference as a confidentiality violation and potential threat to the training data from models. It is recommended in the auditing framework by ICO for organizations to identify these threats and take measures to minimize the risk. As the ICO’s investigation teams will be using this framework to assess the compliance with data protection laws, organizations must account for and estimate the privacy risks to data through models.

ML privacy meter can help in DPIA by providing a quantitative assessment of privacy risk of a machine learning model. The tool can generate extensive privacy reports about the aggregate and individual risk for data records in the training set at multiple levels of access to the model. It can estimate the amount of information that can be revealed through the predictions of a model (referred to as Black-box access) and through both the predictions and parameters of a model (referred to as White-box access). Hence, when providing query access to the model or revealing the entire model, the tool can be used to assess the potential threats to training data.

{:refdef: style="text-align: center;"}
<img src="/assets/2020-09-07-MLPrivacyMeter/privacy_risk.png" width="75%">
{:refdef}

ML Privacy Meter works by implementing membership inference attacks against machine learning models. It simulates attackers with different levels of access and knowledge about the model. It considers attackers who can exploit only the predictions of the model, the loss values, and the parameters of the model. For each of the simulated attacks, the tool reports risk scores for all the data records. These scores represent the attacker’s belief that the record was part of the training dataset. The larger the gap between the distribution of these scores for records that are in the training set versus records that are not in the training set, the larger is the leakage from the model would be. 

{:refdef: style="text-align: center;"}
<img src="/assets/2020-09-07-MLPrivacyMeter/roc.png" width="75%">
{:refdef}

Success of the attacker can be quantified by an ROC curve representing the trade-off between False Positive Rate and True Positive Rate of the attacker. True positive represents correctly identifying a member as present in the data and False positive refers to identifying a non-member as member. An attack is successful if it can achieve larger values of True Positive rate at small values of False Positive rate. A trivial attack such as random guess can achieve equal True Positive and False Positive Rates. ML Privacy Meter automatically plots the trade-offs that are achieved by our simulated attackers. The area under those curves quantifies the aggregate privacy risk to the data posed by the model. The higher the area under curve, larger the risk. These numbers not only quantify the success of membership inference attacks, but they can also be seen as a measure of information leakage from the model.

{:refdef: style="text-align: center;"}
<img src="/assets/2020-09-07-MLPrivacyMeter/privacy_risk_label15.png" width="45%">
<img src="/assets/2020-09-07-MLPrivacyMeter/privacy_risk_label45.png" width="45%"> 
{:refdef} 

When deploying machine learning models, this quantification of risk can be useful while performing a Data Protection Impact Assessment. The aim of doing a DPIA is to analyze, identify and minimize the potential threats to data. ML privacy meter can guide practitioners in all the three steps. The tool produces detailed privacy reports for the training data. It allows comparing the risk across records from different classes in the data. It can also compare the risk posed by providing black box access to the model with the risk due to white box access. As the tool can immediately measure the privacy risks for training data, practitioners can take simple actions such as finetuning their regularization techniques, sub-sampling, re-sampling their data, etc., to reduce the privacy risk. Or they can even choose to learn with a privacy protection, such as differential privacy, in place.  

Differential Privacy is a cryptographic notion of privacy, wherein the outputs of a computation should be indistinguishable when any single record in the data is modified. The level of indistinguishability is controlled by a privacy parameter $\epsilon$. Open source tools such as [OpenDP](https://github.com/opendifferentialprivacy/) and [TensorFlow Privacy](https://github.com/tensorflow/privacy) are available for training models with differential privacy guarantees. Selecting an appropriate value for $\epsilon$ is highly non-trivial when using these tools. Models learned with smaller value of $\epsilon$ provide better privacy guarantees but are also less accurate. $\epsilon$ represents a worst case upper bound on the privacy risk and the practical risk might be much lower. 

ML Privacy Meter can help in the selection of privacy parameters ($\epsilon$) for differential privacy by quantifying the risk posed at each value of epsilon. Compared to just relying on the guarantees provided by epsilon, using this method helps in deploying models with higher accuracy. By letting practitioners choose models with better utility, ML Privacy Meter can enable the use of privacy risk minimization techniques.

## Key takeaways

* By leaking information through predictions and parameters, machine learning models pose an additional privacy risk to data in AI systems.

* To comply with data protection regulations, we need to assess these risks and take possible mitigation measures. 

* ML Privacy Meter quantifies the privacy risk of machine learning models to their training data and can guide practitioners in regulatory compliance by helping them analyze, identify, and minimize the threats to data. Repository for the code and tutorials is available [here](https://github.com/privacytrustlab/ml_privacy_meter)


### References 

<a name="Shokri15"></a> Shokri, Reza, and Vitaly Shmatikov. "Privacy-preserving deep learning." Proceedings of the 22nd ACM SIGSAC conference on computer and communications security. 2015.

<a name="Shokri17"></a> Shokri, Reza, et al. "Membership inference attacks against machine learning models." 2017 IEEE Symposium on Security and Privacy (SP). IEEE, 2017.

<a name="Nasr19"></a> Nasr, Milad, Reza Shokri, and Amir Houmansadr. "Comprehensive privacy analysis of deep learning: Passive and active white-box inference attacks against centralized and federated learning." 2019 IEEE Symposium on Security and Privacy (SP). IEEE, 2019.

<a name="Song17"></a> C. Song, T. Ristenpart, and V. Shmatikov. Machinelearning models that remember too much. InProceedings of the 2017 ACM SIGSAC Conference onComputer and Communications Security, pages587–601, 2017.

<a name="Carlini19"></a> N. Carlini, C. Liu, ́U. Erlingsson, J. Kos, and D. Song.The secret sharer:  Evaluating and testing unintendedmemorization in neural networks. In28thUSENIXSecurity Symposium, pages 267–284, 2019.

<a name="McMahan17"></a> B. McMahan, E. Moore, D. Ramage, S. Hampson, andB. A. y Arcas. Communication-efficient learning ofdeep networks from decentralized data. InArtificialIntelligence and Statistics, pages 1273–1282, 2017

