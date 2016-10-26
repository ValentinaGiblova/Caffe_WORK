# Caffe_WORK

# Рекомендации по установки библиотеки caffe на Windows (64 bits)

1. Скачать и установить Visual Studio 2013 (английский язык) https://www.microsoft.com/ru-ru/download/details.aspx?id=44914
2. Скачать и установить CUDA 7.5 https://developer.nvidia.com/cuda-75-downloads-archive
Скачать CUDA® Deep Neural Network library (cuDNN) для 7.5 https://developer.nvidia.com/cudnn
Скачать framework с github  - caffe https://github.com/BVLC/caffe/tree/windows
Скачать и установить Python 27 https://www.python.org/downloads/
Скачать Miniconda для Python 27 и установить http://conda.pydata.org/miniconda.html
Установить пакеты в Anaconda Prompt при помощи команд
conda install --yes numpy scipy matplotlib scikit-image pip
conda install -c conda-forge protobuf=3.0.0
8. Разархивировать framework caffe
9. В полученной папке caffe скопировать и вставить файл CommonSettings.props.example (файл лежит caffe_root/caffe/windows)
10. Поменять название скопированного файла с  CommonSettings.props.example на  CommonSettings (шаг 9-10 можно посмотреть на видео https://www.youtube.com/watch?v=nrzAF2sxHHM&spfreload=1)
11. Открыть при помощи NotePad файл CommonSettings
12. Заменить в CommonSettings несколько строк
До:
 <UseCuDNN>true</UseCuDNN>
<PythonSupport>false</PythonSupport>
<CudaArchitecture>compute_35,sm_35;compute_52,sm_52</CudaArchitecture>
 <CuDnnPath></CuDnnPath>
<PythonDir>C:\Miniconda2\</PythonDir>
После:
<UseCuDNN>false</UseCuDNN>
<PythonSupport>true</PythonSupport>
<CudaArchitecture>compute_30,sm_30;compute_52,sm_52</CudaArchitecture> (нужно устанавливать в зависимости от capability видеокарты)
 <CuDnnPath>$(SolutionDir)\..</CuDnnPath> (скаченный файл из шага 3 нужно разархивировать и положить  caffe_root/caffe/windows - где лежит и сам файл CommonSettings )
<PythonDir>C:\Users\nasedkina\Anaconda2\</PythonDir> (пропишите путь до корневой папки Miniconda, где хранятся все пакеты для Miniconda) (будьте внимательны со слешами)
13. При помощи VS 2013 открыть файл Caffe.sln
14. Проверить, что произошла загрузка всех проектов
15. Правой кнопкой мыши нажать на Solution Caffe и выбрать Manage NuGet Packages for Solution

16. Во всплывающем окне нажать на кнопку “Restore” на строчке “Some NuGet packages are missing from this solution. Clicks to restore from your online package sources.”
17. В разделе “Installed packages” найти пакет  OpenCV и убрать галки со всем присутствующих проектов. Как результат пакет OpenCV должен исчезнуть из списка Installed Packages
18. Зайти в раздел Online, найти пакет  “Opencv Default Build” и установить его для всех проектов.
19. Перейти в раздел  “Installed packages” и проверить, что установился не только  пакет  “Opencv Default Build”, но и  “Opencv Default Build Redist”
20. Для всего проекта заменить конфигурацию с Debug на Release. Проверить результат: Правой кнопкой мыши нажать на Solution Caffe и выбрать Configuration Manager.
21. Выполнение шагов 13-20 можно посмотреть на https://www.youtube.com/watch?v=nrzAF2sxHHM&spfreload=1
22. Если отображаются ошибки связанные с библиотекой libcaffe - проверьте, что для нее установлены все пакеты из NuGet(Правой кнопкой мыши нажать на Solution Caffe и выбрать Manage NuGet Packages for Solution - “Installed Packages” - для всех установленных пакетов нажать на кнопку “Manage”  и проверить, что все проекты отмечены)
23. Если проблемы связаны с Python, то нужно проверить правильность написанного пути до Miniconda_root (проблема скорее всего в слешах)
24. Если проблема в CUDA 7.5, то проверьте, что  в C:\Program Files (x86)\MSBuild\Microsoft.Cpp\v4.0\BuildCustomizations присутствуют три файла, начинающиеся с CUDA 7.5
25. После успешного построения всего проекта Caffe скопировать папку caffe из caffe_root\caffe\Build\x64\Release\pycaffe и вставить в Miniconda_root\Lib\site-packages
26. открыть Miniconda Prompt  и убедиться, что команда import caffe отработала без ошибок



Применение библиотеки caffe для распознавания изображений из небольшого набора данных с помощью дообучения (fine-tuning) заранее обученной глубокой нейронной сети.

Подготовка данных
скачиваем данные
преобразуем данные в формат lmdb (используемый caffe)
разделяем данные на две выборки: обучающую и валидационную ( train_lmdb and validation_lmbd)
 генерируем mean image
2. Изменяем данные в модели. В работе будет использовать модель bvlc_reference_caffenet prototxt file, которая является модификацией модели AlexNet
Изменить путь к входным данным (строки:24,40,51)
В 373 строке изменить количество входных классов на 101
3. Запустить solver (у файла solver.prototext) изменить пути  к входным данным
4. Обучаем модель (запуск train.prototext)
5. Запускаем обученную модель на новых данных (val.prototext)
Для этого удалим данные слоев, изменим названия слоев и изменим последний тип слоя с SoftmaxWithLoss на Softmax.

 
