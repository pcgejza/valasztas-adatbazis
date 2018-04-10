2018-as választási eredmények SQL-ben
=====================================

Pár példa keresés:

1. Adott szavazókör listát szavazatai

```sql
SELECT tlista.tnev, szavt.szav FROM szavf
  JOIN szavt ON szavf.jfid = szavt.jfid
  JOIN tlista ON szavt.jlid = tlista.tlid
  WHERE szavf.maz = '03’ AND szavf.taz = '004’ AND szavf.sorsz = '009’
  AND szavf.valtip = 'L’
```

2. Adott szavazókör egyéni szavazatai

```sql
SELECT ejelolt.nev, ejelolt.unev1, jlcs.nevt, szavt.szav FROM szavf
  JOIN szavt ON szavf.jfid = szavt.jfid
  JOIN ejelolt ON szavt.jlid = ejelolt.eid
  LEFT JOIN jlcs ON jlcs.jlcs = ejelolt.jlcs
  WHERE szavf.maz = ‘03’ AND szavf.taz = '004’ AND szavf.sorsz = '009’
  AND szavf.valtip = 'J’
```

3. Adott szavazókörben a listára és egyénire adott szavazatok különbsége

```sql
SELECT lista.maz, lista.taz, lista.sorsz, lista.tnev, lista.szav as listaszav, egyen.szav as egyenszav FROM
  (SELECT szavf.maz, szavf.taz, szavf.sorsz, tlista.tnev, szavt.szav FROM szavf
   JOIN szavt ON szavf.jfid = szavt.jfid
   JOIN tlista ON szavt.jlid = tlista.tlid
   WHERE szavf.maz = '03’ AND szavf.taz = '004’ AND szavf.sorsz = '009’
   AND szavf.valtip = 'L’) as lista
 JOIN
  (SELECT szavf.maz, szavf.taz, szavf.sorsz, ejelolt.nev, ejelolt.unev1, jlcs.nevt, szavt.szav FROM szavf
   JOIN szavt ON szavf.jfid = szavt.jfid
   JOIN ejelolt ON szavt.jlid = ejelolt.eid
   LEFT JOIN jlcs ON jlcs.jlcs = ejelolt.jlcs
   WHERE szavf.maz = '03’ AND szavf.taz = '004’ AND szavf.sorsz = '009’
   AND szavf.valtip = 'J’) as egyen
 ON lista.tnev = egyen.nevt AND lista.maz = egyen.maz and lista.taz = egyen.taz and lista.sorsz = egyen.sorsz
```
