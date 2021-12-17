# word_prediction

This  is a code fecth program for the word prediction part of zero to NLP

before deep diving into it, we will have an recap of how Long-Short-Term-Memory(LSTM) works

## LSTM
LSTMs have an edge over conventional feed-forward neural networks and RNN in many ways. This is because of their property of selectively remembering patterns for long durations of time.  The purpose of this article is to explain LSTM and enable you to use it in real life problems.  Let’s have a look!

#### Whats the upper hand LSTM has over vanilla RNN models?

While RNN cannot rememver things for long time.., LSTMs on the other hand, make small modifications to the information by multiplications and additions. With LSTMs, the information flows through a mechanism known as cell states. This way, LSTMs can selectively remember or forget things. The information at a particular cell state has three different dependencies.


We’ll visualize this with an example. Let’s take the example of predicting stock prices for a particular stock. The stock price of today will depend upon:

* The trend that the stock has been following in the previous days, maybe a downtrend or an uptrend.
* The price of the stock on the previous day, because many traders compare the stock’s previous day price before buying it.
* The factors that can affect the price of the stock for today. This can be a new company policy that is being criticized widely, or a drop in the company’s profit, or maybe an unexpected change in the senior leadership of the company.
These dependencies can be generalized to any problem as:

* The previous cell state (i.e. the information that was present in the memory after the previous time step)
* The previous hidden state (i.e. this is the same as the output of the previous cell)
* The input at the current time step (i.e. the new information that is being fed in at that moment)


Another  relatable example is the one with conveyor belts
_We may have some addition, modification or removal of information as it flows through the different layers, just like a product may be molded, painted or packed while it is on a conveyor belt._

So enough of talking lets make a deep dive and understand whats happening behind the scenes


### Architechture of LSTMs
![image](https://user-images.githubusercontent.com/40365028/146511625-eedba2f2-9eaf-4c46-9701-7ce55fbe92c9.png)

  There are two states that are being transferred to the next cell; the cell state and the hidden state. The memory blocks are responsible for remembering things and manipulations to this memory is done through three major mechanisms, called gates. Each of them is being discussed below.

Inorder to have a comprehensive understanding about the working of LSTM, lets know about the gates:
* Forget gates
* Input gates
* output gates

#### Forget Gates

![image](https://user-images.githubusercontent.com/40365028/146531672-5c959b4e-c377-41f7-8708-793e450cf86b.png)

Above is the forget gate in lstm layer

_A forget gate is responsible for removing information from the cell state. The information that is no longer required for the LSTM to understand things or the information that is of less importance is removed via multiplication of a filter. This is required for optimizing the performance of the LSTM network._
input:  h<sub>t-1</sub>(hidden state or output from the current from t - 1) and x<sub>t</sub>(current input)<br/>
calculaton: the given inputs are multiplied by weights and bias is added<br/>
filter: sigmoid function on the calculated vector and a vector values between 0 and 1 is generated<br/>
output: <br/>
0 == > completely delete the context or state
1 == > keep the context or state
the output vector from the sigmoid funtion is multiplied with cell state

#### Input Gates

![image](https://user-images.githubusercontent.com/40365028/146532916-6881e2f0-19ee-4d10-8e4e-646b5c71c523.png)

Above is the forget gate in lstm layer

_The input gate is responsible for the addition of information to the cell state. This addition of information is basically three-step process as seen from the diagram above._

* Regulatory filter: This is basically very similar to the forget gate and acts as a filter for all the information from h_t-1 and x_t.
* Creating a vector containing all possible values that can be added (as perceived from h_t-1 and x_t) to the cell state. This is done using the tanh function, which outputs values from -1 to +1.  
* Multiplying the value of the regulatory filter (the sigmoid gate) to the created vector (the tanh function) and then adding this useful information to the cell state via addition operation.

#### Output Gates

![image](https://user-images.githubusercontent.com/40365028/146535506-769e0aa3-4648-4a8e-93fe-b7c30db051ac.png)

Steps in output gate:
* tanh(cell state)
* Making a filter using the values of h_t-1 and x_t, such that it can regulate the values that need to be output from the vector created above. This filter again employs a sigmoid function.
* Multiplying the value of this regulatory filter to the vector created in step 1, and sending it out as a output and also to the hidden state of the next cell.



