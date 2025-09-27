# Facial-Expression-Analyzer

Short Report: Facial Expression Recognition
1. Network Details 
MobileNet
•	Architecture: Lightweight CNN with depthwise separable convolutions.
•	Parameters: ~4.2M trainable parameters.

Training Setup:
 
•	Optimizer: Adam
•	Learning rate: 1e-4
•	Loss: Combined classification loss (categorical cross-entropy) + regression loss (MAE for valence/arousal).
•	Batch size: 32
•	Epochs: 20

ResNet50
•	Architecture: Deep CNN with 50 layers, skip connections to avoid vanishing gradients.
•	Parameters: ~25.6M trainable parameters.
	Training Setup:
•	Optimizer: Adam
•	Learning rate: 1e-4
•	Loss: Same as MobileNet (classification + regression).
•	Batch size: 32
•	Epochs: 10

Rationale for choosing baselines: MobileNet was selected as a lightweight baseline suitable for resource-constrained environments. ResNet50 was chosen as a deeper model with higher representational power to compare performance trade-offs between efficiency and accuracy.
2. Baseline Comparison 
MobileNet:
•	Training loss decreased smoothly; validation accuracy fluctuated.
•	Better at classifying “Happy” and “Neutral” but struggled with minority classes (e.g., “Fear” and “Disgust”).
•	 Confusion matrix shows misclassifications among similar emotions (e.g., “Anger” vs “Disgust”).
ResNet50:
•	Faster convergence; validation loss decreased more consistently.
•	Confusion matrix shows stronger diagonal dominance (clearer correct predictions).
•	 Significantly higher accuracy in minority classes (Fear, Anger).


Observation: ResNet50 outperformed MobileNet overall, but at the cost of higher computational requirements.

3. Transfer Learning 

Both models were initialized with ImageNet-pretrained weights. Transfer learning accelerated convergence and improved generalization compared to training from scratch, as the models already had learned low-level features (edges, textures) useful for facial recognition.

4. Training Graphs 
MobileNet (20 epochs):
•	Loss steadily decreased, but validation curves showed fluctuations, indicating slight overfitting.
•	Valence/Arousal MAE decreased gradually.

ResNet50 (10 epochs):
•	Loss curves showed smoother convergence.
•	Validation metrics aligned more closely with training metrics, indicating better generalization.


5. Performance Measures & Continuous Domain Evaluation 
•	RMSE (Root Mean Squared Error): Measures deviation in regression tasks (valence/arousal prediction). Useful for penalizing large prediction errors.
•	 CORR (Correlation Coefficient): Measures linear correlation between predicted and true values. Suitable when relative ordering matters more than absolute values.
•	 SAGR (Sign Agreement): Focuses on whether predictions are above or below the mean (correct sign of valence/arousal). Useful for applications requiring sentiment polarity.
•	 CCC (Concordance Correlation Coefficient): Combines correlation with accuracy of mean/variance — most reliable for continuous affective computing tasks.

Most suitable for real-world ("in the wild") systems: CCC is the most comprehensive, as it accounts for both correlation and mean deviation. In uncontrolled environments, CCC ensures the system predicts both the correct trend and magnitude of emotions.

6. Example Predictions

•	Correct Predictions:
  Both MobileNet and ResNet50 correctly classified common classes like “Happy” and “Neutral,” as shown in provided images.

•	Incorrect Predictions:
  Frequent misclassifications occurred between “Fear” and “Surprise” or “Anger” and “Disgust,” highlighting difficulty in distinguishing subtle expressions.

