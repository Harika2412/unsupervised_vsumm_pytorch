This repository contains the implementation of Video Summarization using Reward function.
 The software requirements are: pytorch and python (3.8.2) and also some other like tabulate,h5py etc.
1.Download the dataset(SumMe)
 https://gyglim.github.io/me/vsum/index.html
To download the data set required for the model:
https://github.com/Harika2412/datasets_vsumm.git

Go to the directory where you have downloaded the code.
Then follow the commands given below.

2. First we need to randomly split the dataset
   python create_split.py -d datasets/eccv16_dataset_summe_google_pool5.h5 --save-dir datasets --save-name summe_splits  --num-splits 5
 
3. Train your model
    python main.py -d datasets/eccv16_dataset_summe_google_pool5.h5 -s datasets/summe_splits.json -m summe --gpu 0 --save-dir log/summe-split0 --split-id 0 --verbose
 
  After training your model will be saved in log>summe-split() with a name like: model_epoch50.pth
 You need to copy that path say: log/summe-split0\model_epoch50.pth.tar and then replace "path_to_your_model.pth.tar" in the next command.
 

4. Test your model
    python main.py -d datasets/eccv16_dataset_summe_google_pool5.h5 -s datasets/summe_splits.json -m summe --gpu 0 --save-dir log/summe-split0 --split-id 0 --evaluate --resume path_to_your_model.pth.tar --verbose --save-results
    The output result will be saved to result.h5

    For visualising score vs gtscore you can:
python visualize_results.py -p path_to/result.h5
 Give the correct path for result.h5

    To plot the overall rewards:
 python parse_log.py -p path_to/log_train.txt
 Give the correct path for log_train.txt

    To plot the epoch-reward curve
  python parse_json.py -p path_to/rewards.json -i 0

5. To visualise the summary
  
   Now you need to create a folder named video_frames in the summe-split(). 
   In this folder you should copy the frames of any video for which you want to find the summary.
    Can use command like:
      ffmpeg -i Air_Force_One.webm -r 25 frames\Air_Force_One\%06d.jpg
    Also make sure that the video_frames is inside the summe-split() folder.

 python summary2video.py -p path_to/result.h5 -d path_to/video_frames -i 0 --fps 30 --save-dir log --save-name summary.mp4

 Corresponding video summary will be created in the log directory.

