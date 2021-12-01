# GCN_detect_bitcoin_money_laundering
This project attempts the use new techniques in the field of AML applied to the bitcoin blockchain.

## Discussion points 

__time step__

The papaer commits to a controversial decision in data separation. Briefly, the paper uses time step to separate train, validation and test. The general idea behind this is to track the money trough time, using the past to predict the future. Also in the end they tried using a recurrent layer, which worked with the time step. 
However this choice brings some additional difficulties, as the distribution of the classes vary wildly between the different sets, making training the network very inefficient, as the metrics used on train/validation may be inappopriate for the test set. 

Upon testing, using a split random and proportional with the classes proved to be a much better choice in terms of capabilty of the models, ease of training, reproducibility of results. Its possible to argue that considering the task of financial forensiscs, it really doesnt make any sense tu split with the time step, as we're not trying the predict the illegal transaction of the future as we are analizying the transaction of the past. 

However, I decided to stick with the proposed decision, as doing otherwise would have made impossible to compare my results with the paper ones, therefore making this all project less valuable.

__the tracker__

The tracker algorithm is a novelty and its not present in the original paper. The graph of transaction nodes is vast, and difficult to grasp quickly. After the successfull classification, we may check for the class distribution to have an idea on how the graph is classified, or ,as the paper suggested we may sample the graph and print the given subgraph. 

However, the sampled subgraph is often non-connected, and can give no interesting clue of the classified graph. Therefore I proceeded to develop the tracker function, which is a graph traversing algorithm. The user can set a transaction and the algorithm recursively checks the graph for nodes connected to the given one, and recursively checks the found nodes in the same way. 
What we fin at the end is a fully connected graph, that we can compare in the classified/unclassified states, making a much more viable tool for evaluation.

The developement of this tool pronted me to investigate the possibility of another approach to the graph, by using PPI based algorithm to classify whole graphs rather than single transactions. 

__the GCN architecture__

The gan architecture is a state of the art implementation used also for other graph problems, it is based on the paper "Design Space for Graph Neural Networks"

__the GAT architecture__

The GAT architecture is a novel design, but very primitive. Its based on 2 attention layers and plenty of regularization, as the tendency of exploding gradient was the biggest issue. 
For both GCN and GAT the limited disposal of high performance GPU prevented me to achieve a perfect training and they could both be highly improved, in both terms of tuning and architecture change. 

-----------------------------------------------------------------------------------

In this project we have the following goals:

1. Achieve higher meaning representation of the transactions of the Elliptic blockchain dataset. Produce a transaction tracker that creates graphs making it possible to inspect viasually the performance of the classifier 
2. Reproduce the techniques proposed in the paper hoping to achieve comparable results. 
3. Work on some new state of the art convoluted networks and compare it to the original 
4. Attempt the making of an attention layer in the GCN (GAT) and compare the results with the original paper. 

__fig.1 example of unclassified connected group__

red -> illicit 

green -> licit 

gray -> unclassified 

![unclassified connecet group](https://github.com/fmerizzi/GCN_detect_bitcoin_money_laundering/blob/main/connected%20group.png)

this project is based on the paper "Anti-Money Laundering in Bitcoin: Experimenting with Graph Convolutional Networks for Financial Forensics" by Mark Weber, Giacomo Domeniconi, Jie Chen, Daniel Karl I. Weidele, Claudio Bellei, Tom Robinson, Charles E. Leiserson

### In a possible follow up its planned to explore the follwing concepts:

1. Produce a new dataset containing connected graphs, obtained by searching the dataset 
2. Apply some GAT techniques to classify the obtained connected graphs, with the general idea to find "washing machine" addresses used for money laundering 

### Final Results 
![final performance table](https://github.com/fmerizzi/GCN_detect_bitcoin_money_laundering/blob/main/classification_table.png)
![final performance overview](https://github.com/fmerizzi/GCN_detect_bitcoin_money_laundering/blob/main/final_metrics_classifier.png)


