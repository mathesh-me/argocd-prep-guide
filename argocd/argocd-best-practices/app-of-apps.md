## App of Apps

- If there 100 applications in our cluster, It's not easy to apply `kubectl apply -f` for each application separtely. And there will be a case we might add a new application and we have to come back to the cluster and apply the new application manifest.
- So the best practice is to have an `App of Apps` which will manage all the applications in the cluster.
- We have to **create One App it will deploy all the other applications in the cluster**.

### Way to Implement App of Apps

- Root App tracking the **directory of all the other applications with recursive enabled**.
- Root App tracking a **Helm Chart that include all the other applications**.

Date of Creation: 28/04/2024