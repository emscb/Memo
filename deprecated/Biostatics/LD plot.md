1. Start R
2. MAP 파일을 이용해 locus information 파일을 만든다.
   1. `cg.map <- read.table("[path_to]/cg.map");`
   2. `write.table(cg.map[,c(2,4)],"[path_to]/cg.hmap", col.names=FALSE, row.names=FALSE, quote=FALSE);`
3. Haploview를 켠다. (`java -jar Haploview.jar`)
4. LINKAGE Format 탭에서 Data File에 PED 파일을 넣는다. (e.g. `cg.ped`)
5. Locus Information File에 위에서 만든 `cg.hmap` 파일을 넣고 실행

