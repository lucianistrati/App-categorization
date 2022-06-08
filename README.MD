# App Categorization
You are given:
- millions of apps (each with title, description, icon)
- thousands of apps categories("Soccer Games", "Gardening Apps")

● Proposed approach
- I would make a classification algorithm that can receive an app as datapoint (title, description and icon) and a category as label
- For the feature extraction of this app categorization problem I would likely make a multimodal neural network which receives three inputs (title, description and icon). Title and description would pass through text transformers for feature extraction (something like a "bert-cased" or "bert-uncased"), while the image would pass through a vit(visual transformer) for feature extraction and then all 3 feature extraction outputs would likely be concatenated and further passed through a fully 
  connected neural network which has on the output layer N nodes (with N being the number of classes)
- If not opting for a multimodal approach and for a simple one sided approach, the a GPT2 model could be used for image captioning (meaning that we give it an icon and we receive a text description of what objects are found in the icon and how do they relate spatially). After this preprocessing step we could concatenate the title and the description with the captioning and then pass it all through a text transformer like bert cased or bert uncased with a classifying head directly in its own 
  arhitecture, without adding new components to it

● Measuring performance
- I would calculate several metrics like: accuracy, precision, recall, f1-score. It does not make sense to calculate a confusion matrix because of the high number of classes is hard to see which ones are most oftenly confused (perhaps it would make sense just to calculated and to print just the top 10 or top 50 pairs of classes that get confused most often, meaning that we would choose the elements in the confusion matrix which are not on the principal diagonal).

● Adding/removing a category
- In case of adding a new category, I would use a separate pretrained text similarity model in order to compare the new category to the existing category and see if it would perhaps make more sense to label the new data with already existing labels (if the highest semantic similarity between the new and the existing category is very high >0.95 let's say). If the similarity is not that high nor it would make sense to combine the two, then new data with a new label will simply be added 
  (however it would be important to try this, because it's not really scalable to train and clasify with thousands of labels, nor it yields you a great performance, thus trying to put data under "more or less" the same "umbrella" gives you better results)
- In case of removing a category, I would also look at the semantic similarity of the category name and perhaps the semantic similarity of the descriptions/titles in general.
- I would test semantic similarity of the title and category and/or image similarity of the icons

Observations:
* in order to efficiently compute the semantic similarity I would also to try to clusterize the data using this semantic similarity metric. An even better idea would be to create an average of the embeddings of a class and compare the averaged out embeddings instead of computing each and every one of them separately. 
