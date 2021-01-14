# Machine learning for Microcontrollers

This project intends to provide complete pipelines for sensors data classification and models implementation on small and tiny devices (made with Arduino BLE 33).

The pipeline:

Data creation -> Features extraction -> Features classification -> model implementation on MCU

* high data logging for AVR microcontrollers
* Data acquisition with trigger and save in .csv files on Arduino (DAQ tool)
* Custom Features extraction and spectrogram features extraction
* Custom features classification and spectrogram classification
* Deploy model on MCU for inference on single-value sensor
* Deploy model on MCU for inference on buffer until 600 sensor’s values

# Deploy model on Arduino

For more explanations, please refer to TinyML.pdf (*TinyML: Machine Learning with TensorFlow Lite on Arduino and Ultra-Low-Power Microcontrollers, Book by Daniel Situnayake and Pete Warden*).

## Simple model (single-value inferences)

The project **simple_model.ino** purpose to have a first and simple view on how to develop a model on Arduino. This model intend to receive a sensor value (a single-value) and make an inference on this sensor value.

You can store your model in a model.h file, choose your CLASSES and start to infer!

## Classification on buffer (600 values buffer inferences)

More complex, this project have all the pipeline to deploy a model on Arduino BLE 33 for inference on buffer until 600 sensor’s values.
It includes different parts to make it works:

* `sensor_model_data` to store the model
* `arduino_sensor_handler` to capture 600 values per buffer to infer
* `arduino_output_handler` to print the results
* `Classification_raw_data`, main program
* `sensor_predictor` reduce the impact of false positives by including a minimum threshold and certain
number of inferences to declare a value positive.

# Feature extraction

## Custom feature extraction

## spectrogram features extraction


# features classification for custom model

Aims to classify label based on multi dimension features. Note that these features could be raw data.
A new model can be create in model_fc.py and it's possible to train, evaluate and infer on a model thanks to the command lines bellow.

Command line example:

* To train a new model

    	python features_classification_model.run.py --features_path 110_features_new_data.csv --ignore_columns 6 \
    	--model train --mode evaluate,infer \
    	--lite True --lite_output model_quantized_classifier.tflite


* To load an existing model

    	python features_classification_model.py -features_path 110_features_new_data.csv -ignore_columns 6 \
    	--model load --model_input saved_model/saved_model.h5 \
    	--mode evaluate,infer \
    	--lite True --lite_output saved_model/model_quantized_classifier.tflite

Check [Command Line Args Reference](#Command-Line-Args-Reference) to see all the options.

### Create a new model

Go to model_fc.py, you can modify the model's layers in __init__ method.

Example:

        self.model = tf.keras.models.Sequential([
            tf.keras.layers.Dense(self.node1, input_dim=num_features, activation='relu'),
            tf.keras.layers.Dense(self.node2, activation='relu'),
            tf.keras.layers.Dense(1, activation='sigmoid'), # Output between 0 and 1      
        ])


### Convert .tflite to .cc

* Install xxd if it is not available

        !apt-get -qq install xxd

* Save the file as a C source file

        !xxd -i model_quantized.tflite > model_quantized.cc

###or convert .tflite to arduino header .h

	!echo "const unsigned char model[] = {" > /content/model.h
	!cat gesture_model.tflite | xxd -i      >> /content/model.h
	!echo "};"                              >> /content/model.h

### Command Line Args Reference

    features_classification_model.run.py:
    --features_path: path to features.csv
        (default: '')
    --ignore_columns: first columns to ignore, often to ignore categorical columns
        (default: 0)
        (an integer)
    --model: 'train' or 'load' your model
        (default: 'train')
    --model_input: input path to .h5 saved model file (weight) to load
        (default: '')
    --BATCH_SIZE: batch size
        (default: 32)
        (an integer)
    --NUM_EPOCHS: number of epochs
        (default: 50)
        (an integer)
    --BATCH_SIZE: batch size
        (default: 32)
        (an integer)
    --model_output: output path to save .h5 model file (weight)
        (default: 'saved_model/saved_model.h5')
    --mode: 'evaluate' and/or 'infer' (to convert)
        (default: 'evaluate,infer')
    --lite: convert, save and infer to tensorflow Lite
        (default: False)
        (an boolean)
    --lite_output: output path to save .tflite model file (weight)
        (default: 'model_quantized_classifier.tflite')
