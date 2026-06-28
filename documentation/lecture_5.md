Model serving architectures
1. Batch serving - all predictions are computed and stored in db.
2. Real time model serving - ML endpoint is computing predictions at the request.
3. Feature serving
4. Model serving endpoint needs to registered  model in unity catalog
5. You have a model artifact and need predictions to be computed at the moment of request -> use model serving
6. You have a model artifact, and need predictions to be computed and some features to be accessed at the moment of request -> use model serving with feature lookup
7. Your predictions are pre-computed and need to be accessed at the moment of request -> use feature serving
8. You need to compute a simple function that does not require a model artifcat at the moment of request -> use feature serving