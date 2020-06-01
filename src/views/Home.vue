<template>
  <div class="section">
    <h1 class="title is-1">Welcome to Dog Breed Identification.</h1>
    <img src="@/assets/Dog_CTA_Desktop_HeroImage.jpg" />
    <p>
      This project uses computer vision and machine learning techniques to predict dog breeds from images. First,
      we identify dog facial keypoints for each image using a
      convolutional neural network. These keypoints are then
      used to extract features via SIFT descriptors and color histograms. We then compare a variety of classification algorithms, which use these features to predict the breed of the
      dog shown in the image. Our best classifier is an SVM with
      a linear kernel and it predicts the correct dog breed on its
      first guess 52% of the time; 90% of the time the correct dog
      breed is in the top 10 predictions.
      On this site you can upload any image of a dog and we'll identify it's breed for you. P.S. We don't like cats.
    </p>
  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: "Home"
};
</script>
