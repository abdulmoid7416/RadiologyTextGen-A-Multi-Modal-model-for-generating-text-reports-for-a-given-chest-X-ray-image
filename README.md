# RadiologyTextGen-A-Multi-Modal-model-for-generating-text-reports-for-a-given-chest-X-ray-image

#### Data
Data: https://openi.nlm.nih.gov/faq#collection
Our dataset contains 3996 radiology reports and 8121 associated X ray images.
The chest x-ray images and narrative radiology reports are sourced from the Indiana University hospital network.
Images are in png and reports are in XML format.

#### Data Pre-processing:
Extracted the IMPRESSIONS text from its XML Tag.
Removed reports not having its corresponding Chest X Ray images.
In our dataset, there are multiple Chest X ray  images associated with one report. So, we duplicated reports to have consistent image and text pairing. 
7470 reports <-> 7470 images.
Extracted unique sentences from the IMPRESSIONS Tag. (1800)


#### METHODOLOGY 
![image](https://github.com/abdulmoid7416/RadiologyTextGen-A-Multi-Modal-model-for-generating-text-reports-for-a-given-chest-X-ray-image/assets/55455312/19de7949-8126-472d-bb58-d958cdb7fdde)

In this project we developed multiple models as can be seen from the above figure. 
Model 1 (Image Encoder) Converts an Image to its Image Embedding/features.
Model 2 (Text Encoder): Converts a text sentence into its embedding.
Model 3 (ImgEmb to TextEmb): Converts Image Embeddings (from model 1) to Text Embeddings.
Model 4 (Text Decoder): Takes Text Embeddings (from model 3) as input and predicts text sentences.

##### Training Steps:
1. We trained a VGG19 based autoencoder on our dataset. Then we used its encoder part to extract features (Image Embeddings) from the entire dataset. 
2. For Text Encoder, we used ClinicalBERT, a pre-trained BERT on clinical model, and extracted Text Embeddings from the Text Sentences. We used these text embeddings as target data for the next step.
3. We developed a CNN model (model 3) that takes image embeddings (from 1) and predicts text embeddings. 
4. We then finally developed a Sequence-to-Sequence model consisting of LSTMs to generate text sentences based on the text embeddings (from step 3).

Once all models are trained with the respective data, we then pass a new chest x-ray image to the image encoder (model 1) to get image embedding which is then passed to model 3 to get text embeddings. Then we give these text embeddings to text decoder (model 4) which then generates the ‘impression’ sentence for that x-ray image.
