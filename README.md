# syncrack_generator
## Description
For further information, see: 
```
@conference{visapp22,
  author={Rodrigo Rill{-}García and Eva Dokladalova and Petr Dokládal.},
  title={Syncrack: Improving Pavement and Concrete Crack Detection through Synthetic Data Generation},
  booktitle={Proceedings of the 17th International Joint Conference on Computer Vision, Imaging and Computer Graphics Theory and Applications - Volume 4: VISAPP},
  year={2022},
  pages={147-158},
  publisher={SciTePress},
  organization={INSTICC},
  doi={10.5220/0010837300003124},
  isbn={978-989-758-555-5},
}
```

If you use this software, we kindly ask you to cite the above paper.

The current repository provides a tool for user-parametrized generation of synthetic images emulating pavement and concrete textures. This tool introduces user-parametrized cracks on the texture, providing an accurate segmentation map of the cracks in the image.

Even though real-life annotated datasets exist, the provided labels tend to be inaccurate because a manual accurate annotation in real-life samples is very costful. The ultimate goal of this generator is to provide a tool for training and testing pixel-accurate crack detection methods/models, using accurate annotations.

User parametrization allows creating diverse datasets with different purposes. 
* By changing the background (e.g. texture smoothness and noisy objects), the user can test the robustness of their methods to different material properties. 
* By changing the crack contrast, the user can test the robustness of their methods to the variation of the cracks' visibility.
* By changing the cracks' width, the user can test the robustness of their methods to different scale cracks.

For further information, run 'python generate_dataset.py -h'

An example of a generated pavement image with default parameters, as well as a crack on that image and its corresponding ground-truth are contained in the "examples" folder.

![alt text](https://github.com/Sutadasuto/syncrack_generator/blob/main/examples/img.jpg?raw=true) ![alt text](https://github.com/Sutadasuto/syncrack_generator/blob/main/examples/gt.png?raw=true)

A dataset of 200 pavement images with cracks and their corresponding ground-truths is provided in the "syncrack_dataset.zip" file (those images were generated with default parameters).

To study the effects of inaccurate annotations in supervised crack detection (both during training and evaluation), we provide a tool to generate noisy annotations from a dataset created with this repository. This user-parametrized attack on the clean annotations is generated by morhpological operations.

The noise aims to look similar to how a human would make the annotations in real life: missing some parts of the cracks (erosion) and drawing the cracks thinner (erosion) or wider (dilation) than reality. 

User parametrization allows creating diverse datasets with different purposes. 
* By changing the noise percentage (i.e. the average ratio of regions attacked per image), the user can quantify the effect of different noise levels on supervised crack detection approaches. 
* By changing the type of noise (e.g. having only dilations), or by changing the probability of certain noise type (e.g. having a higher probability of erosion rather than dilation), the user can quantify the impact of certain noise types on supervised methods.

For further information, run 'python generate_noisy_labels.py -h'

An example of a noisy annotation generated with default parameters is contained in the "examples_weak_labels" folder.

The images show the pavement picture as well as a comparison between the actual ground-truth and the noisy annotation: in the annotations image, the left part is the actual ground-truth, the middle one is the noisy annotation, and the right one is a visual comparison of the overlapping of both; green means true positives, blue means false negatives, and red means false positives.

![alt text](https://github.com/Sutadasuto/syncrack_generator/blob/main/examples_weak_labels/img.jpg?raw=true) ![alt text](https://github.com/Sutadasuto/syncrack_generator/blob/main/examples_weak_labels/gt_comparison.png?raw=true)

A noisy version of the dataset from "syncrack_dataset.zip" is provided in "syncrack_dataset_attacked.zip".

## How to run
### Needed packages
The current code was tested using Python 3.7.6. For user convenience, a conda environment setup file (environment.yml) is provided in this repository.

### Running the image generation program
To run with default parameters, you can simply run
```
python generate_dataset.py
```

This will generate 200 images (with their corresponding ground-truths) with 480x320 pixels size (Width x Height) in a folder called "syncrack_dataset" inside the repository's root.

Inside the folder, a text file with the generation parameters will be saved. Additionally, a csv file with the number of crack (true positives) and background (true negatives) pixels will be created. This file contains the number per image and the total number in the dataset. After counting the number of pixels per dataset, it is also expressed as percentage (this is useful to know the percentage of crack pixels in the whole dataset).

As stated before, this tool provides the option for user-parametrized generation in terms of background and crack properties. For more information, and to see default parameters, run
```
python generate_datase.py -h
```

### Running the annotation noise program

To use with default parameters, run
```
python generate_noisy_labels.py path/to/dataset
```
where 'path/to/dataset' is the path to a folder created by "generate_dataset.py". If the generation script was used with default parameters, the path should be 'syncrack_dataset'.

This command will create two new folders in the repository's root:
* 'syncrack_dataset_attacked' will contain a copy of the database, but replacing the ground truth annotations with the noisy annotations; it will contain also a text file with the parameters used to generate the noise.
* 'syncrack_dataset_attacked_label_comparison' will contain the comparisons of the clean and noisy annotations per image (like in the image above); similarly to "generate_dataset.py", the folder will contain a csv with the true positives and negatives, as well as the false postivies/negatives, with respect to the clean annotations.