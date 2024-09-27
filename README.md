# Benchmarking the GRAPES Deep Learning EEW Model on a Suite of California Earthquakes

## Description
Earthquake Early Warning (EEW) systems are designed to give people a heads-up before strong shaking starts. Traditional systems predict shaking by figuring out the size and location of an earthquake, but this can take a few seconds, potentially delaying the warning. The GRAPES EEW algorithm uses a new approach, powered by deep learning, to make faster and more accurate predictions. It looks at data from seismic sensors and connects them in a network, learning how shaking travels between stations. GRAPES analyzes patterns in the data every 4 seconds and updates predictions in real time.

Originally trained on Japanese earthquake data, we're now testing GRAPES on California earthquakes to see how well it works here. In one test with the 2019 M7.1 Ridgecrest earthquake , the system warned all stations that needed alerts, with warning times ranging from immediate at the epicenter to over 30 seconds for stations further away. This shows the potential for GRAPES to help save lives by giving people more time to take cover. We’re continuing to evaluate it on 40 additional California earthquakes to measure its accuracy and warning times.

The faster and more reliable these systems get, the more time people have to protect themselves before the shaking starts.

## 2019 M7.1 Ridgecrest Earthquake Test
<img width="533" alt="Screenshot 2024-09-27 at 3 43 23 PM" src="https://github.com/user-attachments/assets/10a7b50b-ecfc-4189-937d-238cce06037e">

### Processing and Filtering Seismic Event Data
This section describes the process of filtering and synchronizing seismic event data from the SCEC dataset. I began by loading waveform files from multiple events, organized by year and event ID. I filtered out stations that lacked complete data for the East, North, and Vertical components, ensuring data consistency. Next, I synchronized the traces by aligning their start times and removed low-frequency noise to improve data quality. After processing, I compiled station metadata into a streamlined inventory, focusing on station locations. This process handled over 10,000 traces in the overall test and about 900 traces for the Ridgecrest test, producing high-quality data for further analysis.

<img width="666" alt="Screenshot 2024-09-27 at 3 44 57 PM" src="https://github.com/user-attachments/assets/bfee07ac-3649-464a-b095-b2aeb822021a">

### Preparing Seismic Data for GRAPES Model Input
To prepare the inputs for the model, I first processed the seismic waveform data by cleaning and synchronizing the traces from each station. The traces were organized into groups corresponding to the East, North, and Vertical components, ensuring that each station's data was complete. I then aligned the time windows across stations to maintain temporal consistency and applied quality filters to remove low-frequency noise and artifacts. After cleaning the data, I grouped the waveform traces into 4-second windows with 400 samples each, which resulted in a 4D tensor format that included waveform data from all stations.

For the GRAPES model, the input tensors were designed to capture seismic waveform data in real time, focusing on features extracted from each station's waveform components. The GRAPES model is built on a Graph Convolutional Neural Network (GCNN) structure that leverages information from neighboring seismic stations. Each station node in the graph processes waveform data from its own components, and through the GCNN, it incorporates information from nearby stations to enhance prediction accuracy. The model takes these high-dimensional feature vectors, passes them through multiple layers, and outputs predictions about the intensity of ground shaking, effectively providing early earthquake warnings.

<img width="730" alt="Screenshot 2024-09-27 at 3 56 02 PM" src="https://github.com/user-attachments/assets/33f8217b-a9d3-477c-9151-7689115b212e">
<img width="717" alt="Screenshot 2024-09-27 at 3 56 22 PM" src="https://github.com/user-attachments/assets/67f372d5-5511-4510-95b9-ebbbdc853c43">
<img width="715" alt="Screenshot 2024-09-27 at 3 56 38 PM" src="https://github.com/user-attachments/assets/e390cc89-6bde-40d2-bf30-cc9aacddb6d7">


