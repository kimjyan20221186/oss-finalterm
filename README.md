# oss-finalterm
## project title and motivation
This is explanation of my finalterm project of OpenSW. The project was increasing the accuracy of tumor image(glioma, meningioma, no tumor, pituitary) classification using only sklearn. I tried to use algorithms to improve accuracy with optuna, the hyperparameter tuning framework.
## Code development
I will explain the final submitted file first. I did not touch a given preprocessing process and tried to improve performance by changing only algorithms and hyperparameters. So I imported _KNeighborsClassifier_ and tuned the hyperparameters to improve performance. The parameters I changed in KNeighborsClassifier are **n_neighbors, weights, algorithm, and leaf_size.**
## missing but important code
The combination of these hyperparameters was not obtained manually, but was obtained through the __*optuna*__, A hyperparameter optimization framework, which currently does not have this code in the file.

Because when I first got the combination through Optuna, I erased the process and recorded the code separately. Later, when I tried again to submit the file, the accuracy was lower than that because the combination did not keep coming out, so I thought it was inappropriate to write down what did not come out of the code history, so I just excluded the code itself and just wrote it on README. So it can be imported like:

    import optuna
    def objective(trial):
        n_neighbors = trial.suggest_int("n_neighbors", 1, 50)
        weights = trial.suggest_categorical("weights",['uniform','distance'])
        algorithm = trial.suggest_categorical("algorithm",['kd_tree','ball_tree','brute'])
        leaf_size = trial.suggest_int("leaf_size", 30, 1000)     
        
        classifier_obj = KNeighborsClassifier( n_neighbors = n_neighbors,
                                              weights = weights,
                                              algorithm = algorithm,
                                              leaf_size = leaf_size) 
    
        classifier_obj.fit(X_train, y_train)
        result = round(classifier_obj.score(X_test, y_test) ,5)
    
        return result

    study = optuna.create_study(direction='maximize')
    study.optimize(objective, n_trials = 800)
I also compared the accuracy of the algorithm using *sklearn.utils.discovery.all_estimators* before using optuna. It was also deleted, and if you put it in the file, it will be quite long, so I will simply put it on README.

    from sklearn.utils.discovery import all_estimators
    from sklearn.metrics import accuracy_score
    all= all_estimators(type_filter = 'classifier')
  
    for (name , algorithm) in all:
        try:
            model = algorithm()
            model.fit(X_train,y_train)
      
            y_predict = model.predict(X_test)
            acc = accuracy_score(y_test , y_predict)
            print(name , '의 정답률 : ', acc)
        except:
            continue
## Accuracy
When only with the given data, 0.89 was found, and this file is the same as the file submitted during the second inspection, so the accuracy in the test data is about 0.74.

## Regret
In fact, I tried to improve accuracy using an algorithm **(MLP)** that was different from the second one. I tried to use MLPC classifier by trying to increase the data, but the accuracy became lower, and I thought it would be difficult to increase it in time, so I ended up releasing the second one as it was.

It is regrettable to think that if I had looked more closely at the relationship between data and algorithms, I might have been able to increase the accuracy.
