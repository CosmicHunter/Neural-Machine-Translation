# Neural-Machine-Translation

## Translating human readable dates into machine readable dates

* The model you will build here could be used to translate from one language to another, such as translating from English to Hindi. 
* The network will input a date written in a variety of possible formats (*e.g. "the 29th of August 1958", "03/30/1968", "24 JUNE 1987"*) 
* The network will translate them into standardized, machine readable dates (*e.g. "1958-08-29", "1968-03-30", "1987-06-24"*). 
* We will have the network learn to output dates in the common machine-readable format YYYY-MM-DD. 

## Neural machine translation with attention

* If you had to translate a book's paragraph from French to English, you would not read the whole paragraph, then close the book and translate. 
* Even during the translation process, you would read/re-read and focus on the parts of the French paragraph corresponding to the parts of the English you are writing down. 
* The attention mechanism tells a Neural Machine Translation model where it should pay attention to at any step. 


### Attention mechanism

In this part, you will implement the attention mechanism presented in the lecture videos. 
* Here is a figure to remind you how the model works. 
    * The diagram on the left shows the attention model. 
    * The diagram on the right shows what one "attention" step does to calculate the attention variables ![](http://latex.codecogs.com/gif.latex?%24%5Calpha%5E%7B%5Clangle%20t%2C%20t%27%20%5Crangle%7D%24.)
    * The attention variables ![](http://latex.codecogs.com/gif.latex?%24%5Calpha%5E%7B%5Clangle%20t%2C%20t%27%20%5Crangle%7D%24) are used to compute the context variable ![](http://latex.codecogs.com/gif.latex?%24context%5E%7B%5Clangle%20t%20%5Crangle%7D%24) for each timestep in the output (![](http://latex.codecogs.com/gif.latex?%24t%3D1%2C%20%5Cldots%2C%20T_y%24)). 
    
Here are some properties of the model that you may notice: 

#### Pre-attention and Post-attention LSTMs on both sides of the attention mechanism
* There are two separate LSTMs in this model (see diagram on the left): pre-attention and post-attention LSTMs.
* *Pre-attention* Bi-LSTM is the one at the bottom of the picture is a Bi-directional LSTM and comes *before* the attention mechanism.
    - The attention mechanism is shown in the middle of the left-hand diagram.
    - The pre-attention Bi-LSTM goes through <img src="https://render.githubusercontent.com/render/math?math=Tx"> time steps
* *Post-attention* LSTM: at the top of the diagram comes *after* the attention mechanism. 
    - The post-attention LSTM goes through <img src="https://render.githubusercontent.com/render/math?math=T_y"> time steps. 
* The post-attention LSTM passes the hidden state <img src="https://render.githubusercontent.com/render/math?math=s^{\langle t \rangle}"> and cell state <img src="https://render.githubusercontent.com/render/math?math=c^{\langle t \rangle}"> from one time step to the next.     


#### An LSTM has both a hidden state and cell state
*  we are using an LSTM instead of a basic RNN.
    * So the LSTM has both the hidden state ![](http://latex.codecogs.com/gif.latex?%24s%5E%7B%5Clangle%20t%5Crangle%7D%24) and the cell state ![](http://latex.codecogs.com/gif.latex?%24c%5E%7B%5Clangle%20t%5Crangle%7D%24).

#### Each time step does not use predictions from the previous time step
* Unlike previous text generation examples earlier in the course, in this model, the post-attention LSTM at time $t$ does not take the previous time step's prediction ![](http://latex.codecogs.com/gif.latex?%24y%5E%7B%5Clangle%20t-1%20%5Crangle%7D%24) as input.
* The post-attention LSTM at time 't' only takes the hidden state ![](http://latex.codecogs.com/gif.latex?%24s%5E%7B%5Clangle%20t%5Crangle%7D%24) and cell state ![](http://latex.codecogs.com/gif.latex?%24c%5E%7B%5Clangle%20t%5Crangle%7D%24) as input. 
* We have designed the model this way because unlike language generation (where adjacent characters are highly correlated) there isn't as strong a dependency between the previous character and the next character in a YYYY-MM-DD date.

![](img1.png)
    
