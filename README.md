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

