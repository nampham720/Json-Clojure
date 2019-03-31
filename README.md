# Json-Clojure

Basic working with JSON file in Clojure

**Requirement**: leningen

File name: grades.txt

*project.clj: where to import extra dependency (cheshire for JSON)
*core.clj: main project code. 
```clojure
(ns csci3055u-a3.core
  (:require [cheshire.core :as cc]))

;;Parsing json
(defn parse [file]
  (let [json (slurp file)]
    (cc/parse-string json true)))

;;Get student's grade
(defn get-grade [file] 
  (for [i (get-in file [:students])] (get-in i [:grade])))


;;Calculate avg
(defn avg [file] 
  (double (/ (reduce + (get-grade file)) (count (get-grade file)))))

;;Main
(defn main [file]
  (let [file (parse file)
	best_stud (apply max-key :grade (get-in file [:students])) 
        max (format "%.2f" (double (apply max (get-grade file))))
        min (format "%.2f" (double (apply min (get-grade file))))
        avg (avg file)
        title (get-in file [:title])]
  
  (println 
    (str title "\n" 
          "Range: " min " to " max "\n"
          "Average: " (format "%.2f" avg) "\n"
          "Best student: " (get-in best_stud [:name])))))

```
