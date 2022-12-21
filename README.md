# 3XXXCX
load landsat 8
var image = ee.ImageCollection('LANDSAT/LC08/C01/T1_SR') id anh  ee.ImageCollection(.....)
.filterDate( 'tu ngay ', ' den ngay')
.filterBounds (Roi)
.sort('CLOUD_Cover')
.First();

var visPaaramsTrue = {bands: ['B4', 'B3','B2'], min: 0, max: 3000, gamma: 1.4};
Map.addLayer( image.clip(Roi), visPaaramsTrue,'Landsat 2017')
Map.centerObject( roi, 8 or 10);


\\tao data huan luyen
var lable ='Class';
var bands = [ 'B1',  'B2',  'B3',  'B4',  'B5',  'B7'];
var input = image.select(bands);
var training = Urban.merge(...).merge(...)...merge(....);
print(training);


\\ chong lop mau huan luyen len anh
var tranImage = input.sampleRegions({
collection: training,
properties: [label],
scale: 30
});
print(traninImage);
var trainingData = trainImage.randomColumn();
var trainset = trainingData.filter(ee.Filter.lessThan('random', 0.8));
var testSet = trainingData.filter(ee.Filter.greaterThanorEquals('random', 0.8));


\\Mo hinh Phan loai
var classifier = ee.Classifier.smileCart().train(trainSet, label, bands);

\\Phan loai anh
var classified = input.classify(classifier);
print(classified.getInfo());

\\ Define a palette for the classification
var landcoverPalette= [
 'xxxx',   \\water(0)                         \\xxxx la ma mau ( gia tri khi lay mau)

];
Map.addLayer(classified, {palette: landcoverPalette, min: 0, max: 4, 'classification'});





\\ Accuracy Assessment
\\ Classify the testSet and get a confusion matrix
var confusionMatrix= ee.ConfusionMatrix(testSet.classify(classifier)
.errorMatrix({
  actual: 'Class'
  predicted: 'classification'
}));
print('Confusion Matrix:',confusionMatrix);
print("Overall Accuracy:",confusionMatrix.accuracy())

\\ Luu ban do phan loai den google drive
Export.image.toDrive({
image: classified,
description: 'Landsat_Classified_CART',
scale: 30
region: image.geometry(),
maxPixel: 1e13,
});



var test_training = urban.merge(...).merge(...).merge(...);
print(test_training);

\\Luu mẫu training 
Export.table.toAsset(test_training);
xuat vào Drive 
Export.image.toDrive({
image: image,
description: ' Landsat 2017 ....',               (ten file anh)
scale 30,
region: roi,
maxPixels: 1e13
});

\\tao dataset tranning
var training = image.sample({
region: region,
scale: 30,
mumPixels: 5000
});

var clusterer = ee.Clusterer.wekaKMeans(15).train(training);
 var result = image.cluster(Clusterer);
print("result", result.getInfo());


Display the clusters with ramdom colors

Map.addLayer(result.clip(region).randomVisualizer(),{}, 'Unsupervised Classification');

