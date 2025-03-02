# Инструкция к проекту
Проект по улучшению базовой модели предсказания цен на платформе "Яндекс Недвижимость" c приминением MLflow.


1. Запуск проекта:

        git clone https://github.com/AndrushikB/mle-project-sprint-2-v001.git

        cd mle-project-sprint-2-v001

        pip install -r requirements.txt

   (возможно потребуется дополнительно: pip install psycopg2-binary)

3. Этап 1:

   Запуск MLflow с отдельным Tracking Server, реляционной базой данных и хранилищем для артефактов:
          
          run_mlflow_server.sh
  
   Путь: mle-project-sprint-2-v001/mlflow_server/run_mlflow_server.sh

   Для запуска сервера потребуется добавить в виртуальную среду переменные окружения:

        export $(cat .env | xargs)

   Логирование базовой модели: base_model_registry

   Эксперимент: flats_andreybikmulinvik
   
   id эксперимента: 3

3. Этап 2:

   Ключевые шаги EDA:
   - анализ числовых признаков;
   - анализ категориальных признаков;
   - анализ целевой переменной;
   - совместный анализ данных.
  
   Основные выводы:
   - Признак "studio" абсолютно не информативен, поскольку среди представленных квартир нету студий;
   - Среди почти 20 тысяч экземпляров данных всего два имеют "building_type_int" = 6;
   - Также среди представленных квартир очень мало апартаментов;
   - Цены на недвижимость имеют распределение близкое к нормальному;
   - Самые дорогие дома датой постройки до 1950 года, после 1960 года идет резкий спад.

       Запуск в mlflow: eda 
  
4. Этап 3:

   - К категориальным признакам применен OneHotEncoder (поскольку большинство признаков бинарные, а единственный небинарный имеет числовую природу)
   - К числовым применен ряд трансформеров: SplineTransformer, QuantileTransformer, RobustScaler, PolynomialFeatures, KBinsDiscretizer
  
   Запуск в mlflow: preprocessing

   Логирование модели: model_1_preprocessing

5. Этап 4:

   Отбор признаков производился оберточными методами Sequential Forward Selector и Sequential Backward Selector, далее отбирались пересекшиеся и объединенные признаки по результатам работы двух методов и строилась новая версия модели.

   Запуск в mlflow: feature_selection_intersection_and_union 
   
   Логирование модели: model_2_FS

6. Этап 5:

   Подбор гиперпараметров производился на модели Lasso при помощи байесовского подхода и случайного поиска.

   Запуски в mlflow: bayesian_search, random_search

   Логирование финальной версии модели: model_3_hyperparam


__Все конечные версии запусков в MLflow имеют тэг: Final__
   
