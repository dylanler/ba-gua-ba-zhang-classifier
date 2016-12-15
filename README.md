# ba-gua-ba-zhang-classifier

Here are the steps:
You need to install docker on your machine → Docker Page
Make a directory called tf_files in your home directory and put your images in the directory.

cd ~
mkdir tf_files
When you ls your tf_files you should see your folders of images in it:
cd tf_files
tf_files $ ls
food
tf_files $ cd food
tf_files $ ls
ba_gua ba_zhang
3. Start a docker virtual container (virtual VM) and copy tf_files to your container
docker run -it -v $HOME/tf_files:/tf_files gcr.io/tensorflow/tensorflow:latest-devel
4. Retrieve the training code:
cd /tensorflow
git pull
5. Start training your model:
# In Docker
python /tensorflow/tensorflow/examples/image_retraining/retrain.py \
--bottleneck_dir=/tf_files/bottlenecks \
--how_many_training_steps 500 \
--model_dir=/tf_files/inception \
--output_graph=/tf_files/retrained_graph.pb \
--output_labels=/tf_files/retrained_labels.txt \
--image_dir /tf_files/food
6. Stop docker (press ctrl-D) and get the the classifying and predicting python file:
# ctrl-D to exit Docker and then:
curl -L https://goo.gl/tx3dqg > $HOME/tf_files/label_image.py
7. Restart docker and viola! You can now classify your image:
docker run -it -v $HOME/tf_files:/tf_files  gcr.io/tensorflow/tensorflow:latest-devel
Classify a Ba Gua:
# In Docker
python /tf_files/label_image.py /tf_files/food/ba_gua/pic_001.jpg
Or….classify a Ba Zhang:
# In Docker
python /tf_files/label_image.py /tf_files/food/ba_zhang/pic_001.jpg
8. You will get a result something like this:
For Ba Gua:
ba gua (score = 0.97737)
ba zhang (score = 0.02263)
For Ba Zhang:
ba zhang (score = 0.96333)
ba gua (score = 0.03667)
