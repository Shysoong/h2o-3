列排序
---------------

在Python中使用 ``sort``函数或者在R中使用 ``arrange`` 函数以创建按列升序(默认)或降序排序的新数据帧。注意，当使用 ``sort`` 函数时，原始数据帧不能包含任何字符串列。

如果在排序中只指定了一列，那么最终的结果将根据这一列按升序(默认)或降序排序。但是，如果在排序中指定多个列，则AIR执行如下所述的操作：

假设有两列，X(第一列)和Y(第二列)：
 
 - AIR将对第一个指定列进行排序，在[0,1]这种情况下，X列会被首先排序。类似地，在[1,0]的情况下，Y列将首先排序。
 - AIR将按照指定的顺序对后续列进行排序，但仅对与第一个排序列具有相同值的行进行排序。如果在第一个排序列中没有重复值，则不会对后续列执行排序。

.. tabs::
   .. code-tab:: r R
   
		# Currently, this function only supports `all.x = TRUE`. All other permutations will fail.
		library(h2o)
		h2o.init()
		
		# Import the smallIntFloats dataset
		X <- h2o.importFile("https://s3.amazonaws.com/h2o-public-test-data/smalldata/synthetic/smallIntFloats.csv.zip")
		X
		         C1           C10
		1     68379 -1.618668e+07
		2  67108864  3.276800e+04
		3     32768 -8.709456e+08
		4        32  1.310720e+05
		5 268435456 -2.910033e+01
		6 105383117 -2.397206e+08

		[180000 rows x 2 columns]

		# Sort on the first column only in ascending order (default)
		X_sorted1 <- h2o.arrange(X,C1)
		X_sorted1
		           C1           C10
		1 -1073593184  7.474380e+05
		2 -1073563127 -2.097152e+06
		3 -1073521109  5.110769e+06
		4 -1073416724  2.220942e+06
		5 -1073361973 -5.707598e+00
		6 -1073357712 -4.650334e+03

		[180000 rows x 2 columns] 

		# Sort on both columns in descending order, specifying to sort on C1 first
		X_sorted2 <- h2o.arrange(X, desc(C1),desc(C10))
		X_sorted2
		          C1         C10
		1 1073593184  256.000000
		2 1073521109 -128.000000
		3 1073257966   15.616867
		4 1073072648    1.884208
		5 1072757094  441.816579
		6 1072669626 -512.000000

		[180000 rows x 2 columns] 

		# Sort on the second column in descending order
		X_sorted3 <- h2o.arrange(X, desc(C10))
		X_sorted3
		         C1        C10
		1 321417689 1073662860
		2       448 1073574390
		3        85 1073288384
		4     -4096 1072908385
		5        28 1072890306
		6  -4194304 1072750253

		[180000 rows x 2 columns] 
   
   .. code-tab:: python
   
		import h2o
		h2o.init()
		
		# Import the smallIntFloats dataset
		df1 = h2o.import_file("https://s3.amazonaws.com/h2o-public-test-data/smalldata/synthetic/smallIntFloats.csv.zip")
		df1
		              C1               C10
		----------------  ----------------
		 68379                -1.61867e+07
		     6.71089e+07   32768
		 32768                -8.70946e+08
		    32            131072
		     2.68435e+08     -29.1003
		     1.05383e+08      -2.39721e+08
		350191             21551.4
		  -188                 2.39872e+07
		   493               525.825
		     9.31041e+07      -1.63828e+08

		[180000 rows x 2 columns]

		# Sort on the first column only in ascending order (default)
		sorted_column_indices=[0]
		df2 = df1.sort(0)
		df2
		          C1               C10
		------------  ----------------
		-1.07359e+09  747438
		-1.07356e+09      -2.09715e+06
		-1.07352e+09       5.11077e+06
		-1.07342e+09       2.22094e+06
		-1.07336e+09      -5.7076
		-1.07336e+09   -4650.33
		-1.07326e+09      -1.04858e+06
		-1.07307e+09    8192
		-1.07291e+09      -1.49017
		-1.07291e+09   -9337.5

		[180000 rows x 2 columns]

		# Sort on both columns in descending order, specifying to sort on C1 first
		df3 = df1.sort([0,1], ascending=[False, False])
		df3
		         C1                C10
		-----------  -----------------
		1.07359e+09      256
		1.07352e+09     -128
		1.07326e+09       15.6169
		1.07307e+09        1.88421
		1.07276e+09      441.817
		1.07267e+09     -512
		1.07233e+09     1444.14
		1.07184e+09  -231812
		1.07096e+09        2.00296e+07
		1.07082e+09        5.36871e+08

		[180000 rows x 2 columns]

		# Sort on the second column in descending order
		df4 = df1.sort(1, ascending=False)
		df4
		               C1          C10
		-----------------  -----------
		      3.21418e+08  1.07366e+09
		    448            1.07357e+09
		     85            1.07329e+09
		  -4096            1.07291e+09
		     28            1.07289e+09
		     -4.1943e+06   1.07275e+09
		      6.61688e+06  1.07254e+09
		 -50127            1.07235e+09
		-262144            1.07207e+09
		     55            1.07175e+09

		[180000 rows x 2 columns]
