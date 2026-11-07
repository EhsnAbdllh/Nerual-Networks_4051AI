# Task 1

## First Step

**My hypothesis**: If we use `lineare` active function the final result of the net will also be linear combination of input features; so when the target relation ,which we're to estimating it, is not linear, the net won't be capable of learning it.
Otherwise the `Tanh`, `Sigmoid` and `ReLU` functinos which are nonlinear, are capable of estimating any function, theoritcally.

**Configuration**
| Learning Rate | Regularization | Train:Test | Noise | Batch Size | Epochs |
|:---:|:---:|:---:|:---:|:---:|:---:|
|0.1|None|70:30|0|10|300|

**Results**: 

| Activation | Training Loss | Test Loss |
| :--- | :---: | :---: |
| Linear | 0.496 | 0.497 |
| Tanh | 0.004 | 0.004 |
| Sigmoid | 0.008 | 0.008 |
| ReLU | 0.000 | 0.000 |

As I expected the `Linear` activation just splitted the page by a line. Its loss is about `0.5`, that is, it predicts nearly *random*. The other nonlinear activations in contrast estimated the target function, which is obviously nonlinear, accurately. Among these activations the `ReLU` was look much more powerful to approximiate the target function; its loss got less than `0.01` only after `30` epochs.

## Second Step
**My hypothesis**: I think increasing the learning rate makes the net much faster to learn, needing fewer epochs to get less than `0.1` loss. Although getting faster I think large rates, maybe `1` and more, will decrease the final limit of net accuracy.

**Configuration**
Activation | Regularization | Train:Test | Noise | Batch Size | Total Epochs |
|:---:|:---:|:---:|:---:|:---:|:---:|
Tanh|None|70:30|0|10|1000|

**Results**:
| Learing Rate | Epoch = | `100` | `300` | `700` | `1000` |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 0.001 | | 0.434 | 0.273 | 0.119 | 0.075 |
| 0.003 | | 0.292 | 0.107 | 0.032 | 0.022 |
| 0.01 | | 0.108 | 0.024 | 0.010 | 0.008 |
| 0.03 | | 0.025 | 0.009 | 0.005 | 0.004 |
| 0.1 | | 0.007 | 0.005 | 0.003 | 0.003 |
| 0.3 | | 0.056 | 0.024 | 0.034 | 0.128 |
| 1 | | 0.096 | 0.071 | 0.065 | 0.068 |
| 3 | | 0.082 | 0.150 | 0.066 | 0.066 |

**Discuss**

1. **When did the net become fluctuating?**
    For `rates >= 0.3`.
2. **What was the best range?**
    According to the results the best range for learning rate is within `[0.01, 0.1]`, where the net learns so fast and also very accurate.
3. **Why do the large rates ruin the net accuracy?**
    Large learning rates make it hard to find the exaxt relative minimum of loss function. The loss function will fluctuatre around its relative minimum constantly and we won't reach the best accuracy.


## Third Step

**My hypothesis**: I think using regularization will decrease the net senesitivity to noises, so the net accuracy becomes less fragile on test.

**Configurations**
No. | Learning Rate | Activation | Regularization | Train:Test | Noise | Batch Size | Total Epochs |
|:---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|1|0.1|Tanh|None|50:50|30|10|1000|
|2|0.1|Tanh|L1|50:50|30|10|1000|
|3|0.1|Tanh|L2|50:50|30|10|1000|

**Results**

1.
    |Train Loss|Test Loss|
    |:---:|:---:|
    |0.073|0.261|
2.
    |Regularization Rate|Train Loss|Test Loss|
    |:---:|:---:|:---:|
    |0.001|0.130|0.157|
    |0.01|0.177|0.136|
    |0.1|0.497|0.492|
3.
    |Regularization Rate|Train Loss|Test Loss|
    |:---:|:---:|:---:|
    |0.001|0.113|0.207|
    |0.01|0.172|0.141|
    |0.1|0.497|0.492|

**Discuss**

1. **What Does the gap between train & test loss mean?**
    it means the net became overfitted; it predicts well on train but fails on test.
2. **Was `sparsity` happend? What it means?**
    Using `L1` with `rates >= 0.001` some weightes became zero. It means `L1` removes unimportant features, so the net becomes faster and less overfitted.
3. **Was `shrinkage` happened? What it means?**
    Using `L2` weights became close to each other. It means misleading featurs won't get extera attention from the model, so they won't lead the model to overfitting.

## Fourth Step

**My hypothesis**
According to the circular distribution of points on page, I guess using `X^2` features will help the model to estimate the shape formula well, without using extera hidden layers.

**Configurations**
No. | Learning Rate | Activation | Regularization | Train:Test | Noise | Batch Size | Total Epochs |Hidden Layers|
|:---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|1|0.1|Tanh|None|70:30|0|10|300|1|
|2|0.1|Tanh|None|70:30|0|10|300|0|

**Results**

1.
    |Train Loss|Test Loss|
    |:---:|:---:|
    |0.003|0.011|
2.
    |Train Loss|Test Loss|
    |:---:|:---:|
    |0.000|0.001|

**Discuss**

**Why the second config is able to estimate well without use of extera layers?** As I mentioned in my hypothesis, the distribution of points on pages looks circular, so the seperator line would be like `Ax^2 + By^2 = C`. It means the real formula is a linear combination of `x^2` & `y^2` and the second config is completely capable of estimating it.

# Task 2

## First Step

**K-Nearest Neighbors**

1. `K=1`: Each point even noisy ones will have affect on points around them. It causes the model highly memorize and overfit so. Also memorizing each train point means high variance for test points and high error mostly influenced by the high variance.
2. `K=55`: The prediction model almost uses only the majority class in train, so it increases the bias and also the error wich is mostly influenced by that bias.
3. `K=9`: Here the noisy points have very few affect on prediction and also the broaderline is not that simple to be underfitted.

**LOESS Regression**

1. `Smoothness=1.00`: it causes highly memorization of train set, which means we get more variance and more error mostly influenced by that variance.
2. `Smoothness=0.07`: it causes a very simple curve, hight bias and high error which almost comes from that bias.
3. `Smoothness=0.52`: Here but the test error is at its least, and also the train error is closest to it so the model is less underfitted and overfitted.

**Why is it impossible to make both `bias` and `variance` zero?**

## Second Step

**Active Function**: According to the first step of task 1, inpropriate active functions, for example `linear` one for a nonlinear problem, may leads us to learn nothing at all.

**Learning Rate**: According to second step, very samll learning rates causes very slow learning so the model performance may be poor even with 300 epochs. But in otherhand very large rates cause each noisy point to make a considerable difference in weights, so make the net memorize each point.

**Regularization**: As I mentioned noisy points may make big differences in weights so some weights may become very larger rather than others, but using regularization control this affects and avoid the net to rely on some specific features which the noises forced us to.

**Regularizaiton Rate**: In addition, very large regularization rates may make many of weights almost zero which means the model won't learn anything.Also very samll rates is equal to non-use of it at all, so won't avoid overfitting.

**Noises**: Very noisy data ruin the generalization performance of the model and make the model more overfitted.

**Input Features**: According to the step 4 of task 1, using propriate features to feed model would upgrade the performance, which means in other hand using inpropriate features may lead the model to learn nearly nothing. For example using `X1` and `X1*X2` to feed model decrease the performance.

| Parameter | Underfit | Overfit |
|:---|:---:|:---:|
|Active Function|inpropriate active function|_
|Learning Rate|very small learning rates|Very large learning rates
|Regularization|_|non-use|
|Regularization Rate|very large|very small|
|Nosies|_|high|
|Input Features|inpropriate features|_|

## Third Step

**When the model is good on train, but fails on test?**
Overfitting is best example of this situation, when the model learned and memorized the characteristics of train data, so does well on train, but in test those characteristics simply change and model fails to predict them.

**Why we need `validation` set? When we allowed to use `test` set?**
Our goal is to mesure performance of model on unseen data, so if we test our model on data which the model has already seen and learned about them, it is kinda cheating. So we shouldn't use `test` data until the final step. Instead, we use `validation` set, a part of train, for the examination and imporvement cycle of developing our model.