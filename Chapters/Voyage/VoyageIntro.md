## Voyage
   url: 'http://smalltalkhub.com/mc/estebanlm/Voyage/main';
   configurationOf: 'VoyageMongo';
   loadStable.
    repository: 'github://pharo-nosql/voyage/mc';
    baseline: 'Voyage';
    onConflictUseIncoming;
    load: 'mongo tests'.
    repository: 'github://pharo-nosql/voyage/mc';
    baseline: 'Voyage';
    onConflictUseIncoming;
    load: 'unqlite tests'.
    repository: 'github://pharo-nosql/voyage/mc';
    baseline: 'Voyage';
    onConflictUseIncoming;
    load: 'memory tests'.