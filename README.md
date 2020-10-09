{
 "cells": [
  {
   "cell_type": "code",
   "execution_count": 2,
   "metadata": {},
   "outputs": [],
   "source": [
    "#Define Bussiness Problem\n",
    "memitigasi resiko tingginya turnover karyawan dengan menyeleksi berdasarkan beberapa faktor\n",
    "\n",
    "#goals\n",
    "mencari faktor-faktor yang berkorelasi dengan tingginya turnover rate karyawan\n",
    "\n",
    "#analisa deskriptif\n",
    "tidak ada missing value\n",
    "\n",
    "#insight\n",
    "Current Attrition Rate Perusahaan berada di kisaran 16.12%.\n",
    "\n",
    "Semakin tinggi usia, semakin rendah attrition rate.\n",
    "Tidak terlalu berpengaruh thd performa.\n",
    "\n",
    "Attrition rate karyawan dengan status cerai paling rendah, dikisaran 10.09%.\n",
    "Attrition rate karyawan dengan status menikah dikisaran 12.48%.\n",
    "Attrition rate karyawan dengan status single paling tinggi dikisaran 25.5%.\n",
    "\n",
    "Tempat tinggal karyawan cukup berpengaruh thd attrition rate, jarak rata-rata tempat tinggal:\n",
    "8.91 utk yg tidak resign, 10.63 utk karyawan yg resign.\n",
    "\n",
    "Tingkat pendidikan hanya sedikit pengaruhnya terhadap attrition rate:\n",
    "2.92 utk yg tidak resign, 2.83 utk karyawan yg resign.\n",
    "\n",
    "Kepuasan hubungan hanya sedikit pengaruhnya terhadap attrition rate:\n",
    "Rata-rata karyawan yang tidak resign, memiliki skor hubungan 2.73, dan untuk yg resign 2.59%\n",
    "\n",
    "Kepemilikan stock option menjadi faktor yg cukup tinggi dalam menentukan attrition rate. \n",
    "0.84 poin untuk karyawan yang tidak resign, dan 0.52 poin untuk karyawan yang resign\n",
    "\n",
    "Pengalaman kerja karyawan menjadi faktor yg cukup tinggi dalam menentukan attrition rate.\n",
    "11.8 tahun utk karyawan yg tidak resign, 8.2 tahun utk karyawan yg resign\n",
    "\n",
    "Karyawan dengan worklife-balance yang lebih tinggi, cenderung memiliki attrition rate yg lebih rendah.\n",
    "2.78 poin utk karyawan yang tidak resign, 2.65 poin utk karyawan yg resign\n",
    "\n",
    "Hampir semua faktor diatas, pengaruhnya tidak signifikan terhadap performa kerja\n",
    "\n",
    "\n",
    "#kesimpulan\n",
    "Untuk menekan attrition rate:\n",
    "Lebih baik memilih karyawan dengan latar belakang:\n",
    "    Usia yg tidak terlalu muda\n",
    "    Statusnya bukan single\n",
    "    Bertempat tinggal dekat\n",
    "    Kepuasan hubungan yang bagus\n",
    "    Lebih berpendidikan\n",
    "    Berencana memiliki stock option\n",
    "    Pengalaman kerja yg tinggi\n",
    "    Tingkat work-life-balance yang tinggi"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 1,
   "metadata": {},
   "outputs": [],
   "source": [
    "import pandas as pd\n",
    "import numpy as np\n",
    "import matplotlib\n",
    "import matplotlib.pyplot as plt\n",
    "import seaborn as sns\n",
    "\n",
    "%matplotlib inline\n",
    "olddata = pd.read_csv('employee.csv')"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 3,
   "metadata": {},
   "outputs": [],
   "source": [
    "data = olddata[['Age','Attrition','DistanceFromHome','Education','MaritalStatus','PerformanceRating','RelationshipSatisfaction','StockOptionLevel','TotalWorkingYears','WorkLifeBalance','YearsAtCompany']]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 4,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Age</th>\n",
       "      <th>Attrition</th>\n",
       "      <th>Department</th>\n",
       "      <th>DistanceFromHome</th>\n",
       "      <th>Education</th>\n",
       "      <th>EducationField</th>\n",
       "      <th>Gender</th>\n",
       "      <th>MaritalStatus</th>\n",
       "      <th>PerformanceRating</th>\n",
       "      <th>RelationshipSatisfaction</th>\n",
       "      <th>StockOptionLevel</th>\n",
       "      <th>TotalWorkingYears</th>\n",
       "      <th>WorkLifeBalance</th>\n",
       "      <th>YearsAtCompany</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>41</td>\n",
       "      <td>Yes</td>\n",
       "      <td>Sales</td>\n",
       "      <td>1</td>\n",
       "      <td>2</td>\n",
       "      <td>Life Sciences</td>\n",
       "      <td>Female</td>\n",
       "      <td>Single</td>\n",
       "      <td>3</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>8</td>\n",
       "      <td>1</td>\n",
       "      <td>6</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>49</td>\n",
       "      <td>No</td>\n",
       "      <td>Research &amp; Development</td>\n",
       "      <td>8</td>\n",
       "      <td>1</td>\n",
       "      <td>Life Sciences</td>\n",
       "      <td>Male</td>\n",
       "      <td>Married</td>\n",
       "      <td>4</td>\n",
       "      <td>4</td>\n",
       "      <td>1</td>\n",
       "      <td>10</td>\n",
       "      <td>3</td>\n",
       "      <td>10</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>37</td>\n",
       "      <td>Yes</td>\n",
       "      <td>Research &amp; Development</td>\n",
       "      <td>2</td>\n",
       "      <td>2</td>\n",
       "      <td>Other</td>\n",
       "      <td>Male</td>\n",
       "      <td>Single</td>\n",
       "      <td>3</td>\n",
       "      <td>2</td>\n",
       "      <td>0</td>\n",
       "      <td>7</td>\n",
       "      <td>3</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>33</td>\n",
       "      <td>No</td>\n",
       "      <td>Research &amp; Development</td>\n",
       "      <td>3</td>\n",
       "      <td>4</td>\n",
       "      <td>Life Sciences</td>\n",
       "      <td>Female</td>\n",
       "      <td>Married</td>\n",
       "      <td>3</td>\n",
       "      <td>3</td>\n",
       "      <td>0</td>\n",
       "      <td>8</td>\n",
       "      <td>3</td>\n",
       "      <td>8</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>27</td>\n",
       "      <td>No</td>\n",
       "      <td>Research &amp; Development</td>\n",
       "      <td>2</td>\n",
       "      <td>1</td>\n",
       "      <td>Medical</td>\n",
       "      <td>Male</td>\n",
       "      <td>Married</td>\n",
       "      <td>3</td>\n",
       "      <td>4</td>\n",
       "      <td>1</td>\n",
       "      <td>6</td>\n",
       "      <td>3</td>\n",
       "      <td>2</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>...</th>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1465</th>\n",
       "      <td>36</td>\n",
       "      <td>No</td>\n",
       "      <td>Research &amp; Development</td>\n",
       "      <td>23</td>\n",
       "      <td>2</td>\n",
       "      <td>Medical</td>\n",
       "      <td>Male</td>\n",
       "      <td>Married</td>\n",
       "      <td>3</td>\n",
       "      <td>3</td>\n",
       "      <td>1</td>\n",
       "      <td>17</td>\n",
       "      <td>3</td>\n",
       "      <td>5</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1466</th>\n",
       "      <td>39</td>\n",
       "      <td>No</td>\n",
       "      <td>Research &amp; Development</td>\n",
       "      <td>6</td>\n",
       "      <td>1</td>\n",
       "      <td>Medical</td>\n",
       "      <td>Male</td>\n",
       "      <td>Married</td>\n",
       "      <td>3</td>\n",
       "      <td>1</td>\n",
       "      <td>1</td>\n",
       "      <td>9</td>\n",
       "      <td>3</td>\n",
       "      <td>7</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1467</th>\n",
       "      <td>27</td>\n",
       "      <td>No</td>\n",
       "      <td>Research &amp; Development</td>\n",
       "      <td>4</td>\n",
       "      <td>3</td>\n",
       "      <td>Life Sciences</td>\n",
       "      <td>Male</td>\n",
       "      <td>Married</td>\n",
       "      <td>4</td>\n",
       "      <td>2</td>\n",
       "      <td>1</td>\n",
       "      <td>6</td>\n",
       "      <td>3</td>\n",
       "      <td>6</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1468</th>\n",
       "      <td>49</td>\n",
       "      <td>No</td>\n",
       "      <td>Sales</td>\n",
       "      <td>2</td>\n",
       "      <td>3</td>\n",
       "      <td>Medical</td>\n",
       "      <td>Male</td>\n",
       "      <td>Married</td>\n",
       "      <td>3</td>\n",
       "      <td>4</td>\n",
       "      <td>0</td>\n",
       "      <td>17</td>\n",
       "      <td>2</td>\n",
       "      <td>9</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1469</th>\n",
       "      <td>34</td>\n",
       "      <td>No</td>\n",
       "      <td>Research &amp; Development</td>\n",
       "      <td>8</td>\n",
       "      <td>3</td>\n",
       "      <td>Medical</td>\n",
       "      <td>Male</td>\n",
       "      <td>Married</td>\n",
       "      <td>3</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>6</td>\n",
       "      <td>4</td>\n",
       "      <td>4</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>1470 rows × 14 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "      Age Attrition              Department  DistanceFromHome  Education  \\\n",
       "0      41       Yes                   Sales                 1          2   \n",
       "1      49        No  Research & Development                 8          1   \n",
       "2      37       Yes  Research & Development                 2          2   \n",
       "3      33        No  Research & Development                 3          4   \n",
       "4      27        No  Research & Development                 2          1   \n",
       "...   ...       ...                     ...               ...        ...   \n",
       "1465   36        No  Research & Development                23          2   \n",
       "1466   39        No  Research & Development                 6          1   \n",
       "1467   27        No  Research & Development                 4          3   \n",
       "1468   49        No                   Sales                 2          3   \n",
       "1469   34        No  Research & Development                 8          3   \n",
       "\n",
       "     EducationField  Gender MaritalStatus  PerformanceRating  \\\n",
       "0     Life Sciences  Female        Single                  3   \n",
       "1     Life Sciences    Male       Married                  4   \n",
       "2             Other    Male        Single                  3   \n",
       "3     Life Sciences  Female       Married                  3   \n",
       "4           Medical    Male       Married                  3   \n",
       "...             ...     ...           ...                ...   \n",
       "1465        Medical    Male       Married                  3   \n",
       "1466        Medical    Male       Married                  3   \n",
       "1467  Life Sciences    Male       Married                  4   \n",
       "1468        Medical    Male       Married                  3   \n",
       "1469        Medical    Male       Married                  3   \n",
       "\n",
       "      RelationshipSatisfaction  StockOptionLevel  TotalWorkingYears  \\\n",
       "0                            1                 0                  8   \n",
       "1                            4                 1                 10   \n",
       "2                            2                 0                  7   \n",
       "3                            3                 0                  8   \n",
       "4                            4                 1                  6   \n",
       "...                        ...               ...                ...   \n",
       "1465                         3                 1                 17   \n",
       "1466                         1                 1                  9   \n",
       "1467                         2                 1                  6   \n",
       "1468                         4                 0                 17   \n",
       "1469                         1                 0                  6   \n",
       "\n",
       "      WorkLifeBalance  YearsAtCompany  \n",
       "0                   1               6  \n",
       "1                   3              10  \n",
       "2                   3               0  \n",
       "3                   3               8  \n",
       "4                   3               2  \n",
       "...               ...             ...  \n",
       "1465                3               5  \n",
       "1466                3               7  \n",
       "1467                3               6  \n",
       "1468                2               9  \n",
       "1469                4               4  \n",
       "\n",
       "[1470 rows x 14 columns]"
      ]
     },
     "execution_count": 4,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<matplotlib.axes._subplots.AxesSubplot at 0x21cca386310>"
      ]
     },
     "execution_count": 5,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 144x144 with 2 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "plt.figure(figsize=(2,2))# untuk size\n",
    "sns.heatmap(data.isna()) # untuk plot heatmap NaN\n",
    "\n",
    "#checking missing value"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 6,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Age                         0\n",
       "Attrition                   0\n",
       "Department                  0\n",
       "DistanceFromHome            0\n",
       "Education                   0\n",
       "EducationField              0\n",
       "Gender                      0\n",
       "MaritalStatus               0\n",
       "PerformanceRating           0\n",
       "RelationshipSatisfaction    0\n",
       "StockOptionLevel            0\n",
       "TotalWorkingYears           0\n",
       "WorkLifeBalance             0\n",
       "YearsAtCompany              0\n",
       "dtype: int64"
      ]
     },
     "execution_count": 6,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data.isna().sum()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 7,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Age</th>\n",
       "      <th>DistanceFromHome</th>\n",
       "      <th>Education</th>\n",
       "      <th>PerformanceRating</th>\n",
       "      <th>RelationshipSatisfaction</th>\n",
       "      <th>StockOptionLevel</th>\n",
       "      <th>TotalWorkingYears</th>\n",
       "      <th>WorkLifeBalance</th>\n",
       "      <th>YearsAtCompany</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>count</th>\n",
       "      <td>1470.000000</td>\n",
       "      <td>1470.000000</td>\n",
       "      <td>1470.000000</td>\n",
       "      <td>1470.000000</td>\n",
       "      <td>1470.000000</td>\n",
       "      <td>1470.000000</td>\n",
       "      <td>1470.000000</td>\n",
       "      <td>1470.000000</td>\n",
       "      <td>1470.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>mean</th>\n",
       "      <td>36.923810</td>\n",
       "      <td>9.192517</td>\n",
       "      <td>2.912925</td>\n",
       "      <td>3.153741</td>\n",
       "      <td>2.712245</td>\n",
       "      <td>0.793878</td>\n",
       "      <td>11.279592</td>\n",
       "      <td>2.761224</td>\n",
       "      <td>7.008163</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>std</th>\n",
       "      <td>9.135373</td>\n",
       "      <td>8.106864</td>\n",
       "      <td>1.024165</td>\n",
       "      <td>0.360824</td>\n",
       "      <td>1.081209</td>\n",
       "      <td>0.852077</td>\n",
       "      <td>7.780782</td>\n",
       "      <td>0.706476</td>\n",
       "      <td>6.126525</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>min</th>\n",
       "      <td>18.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>3.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>0.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>25%</th>\n",
       "      <td>30.000000</td>\n",
       "      <td>2.000000</td>\n",
       "      <td>2.000000</td>\n",
       "      <td>3.000000</td>\n",
       "      <td>2.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>6.000000</td>\n",
       "      <td>2.000000</td>\n",
       "      <td>3.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>50%</th>\n",
       "      <td>36.000000</td>\n",
       "      <td>7.000000</td>\n",
       "      <td>3.000000</td>\n",
       "      <td>3.000000</td>\n",
       "      <td>3.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>10.000000</td>\n",
       "      <td>3.000000</td>\n",
       "      <td>5.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>75%</th>\n",
       "      <td>43.000000</td>\n",
       "      <td>14.000000</td>\n",
       "      <td>4.000000</td>\n",
       "      <td>3.000000</td>\n",
       "      <td>4.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>15.000000</td>\n",
       "      <td>3.000000</td>\n",
       "      <td>9.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>max</th>\n",
       "      <td>60.000000</td>\n",
       "      <td>29.000000</td>\n",
       "      <td>5.000000</td>\n",
       "      <td>4.000000</td>\n",
       "      <td>4.000000</td>\n",
       "      <td>3.000000</td>\n",
       "      <td>40.000000</td>\n",
       "      <td>4.000000</td>\n",
       "      <td>40.000000</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "               Age  DistanceFromHome    Education  PerformanceRating  \\\n",
       "count  1470.000000       1470.000000  1470.000000        1470.000000   \n",
       "mean     36.923810          9.192517     2.912925           3.153741   \n",
       "std       9.135373          8.106864     1.024165           0.360824   \n",
       "min      18.000000          1.000000     1.000000           3.000000   \n",
       "25%      30.000000          2.000000     2.000000           3.000000   \n",
       "50%      36.000000          7.000000     3.000000           3.000000   \n",
       "75%      43.000000         14.000000     4.000000           3.000000   \n",
       "max      60.000000         29.000000     5.000000           4.000000   \n",
       "\n",
       "       RelationshipSatisfaction  StockOptionLevel  TotalWorkingYears  \\\n",
       "count               1470.000000       1470.000000        1470.000000   \n",
       "mean                   2.712245          0.793878          11.279592   \n",
       "std                    1.081209          0.852077           7.780782   \n",
       "min                    1.000000          0.000000           0.000000   \n",
       "25%                    2.000000          0.000000           6.000000   \n",
       "50%                    3.000000          1.000000          10.000000   \n",
       "75%                    4.000000          1.000000          15.000000   \n",
       "max                    4.000000          3.000000          40.000000   \n",
       "\n",
       "       WorkLifeBalance  YearsAtCompany  \n",
       "count      1470.000000     1470.000000  \n",
       "mean          2.761224        7.008163  \n",
       "std           0.706476        6.126525  \n",
       "min           1.000000        0.000000  \n",
       "25%           2.000000        3.000000  \n",
       "50%           3.000000        5.000000  \n",
       "75%           3.000000        9.000000  \n",
       "max           4.000000       40.000000  "
      ]
     },
     "execution_count": 7,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data.describe()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<matplotlib.axes._subplots.AxesSubplot at 0x21cca4b5880>"
      ]
     },
     "execution_count": 8,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "sns.boxplot(data=data,palette='rainbow',orient='h')"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 9,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<matplotlib.axes._subplots.AxesSubplot at 0x21cca60b4c0>"
      ]
     },
     "execution_count": 9,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 432x288 with 2 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "sns.heatmap(data.corr())"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 10,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Yes : 237\n",
      "No : 1233\n",
      "Attrition Rate: 16.122448979591837 %\n"
     ]
    }
   ],
   "source": [
    "att_total = data['Attrition'].value_counts()\n",
    "\n",
    "#company total attrition rate\n",
    "att_rate = att_total['Yes']/att_total.sum()*100\n",
    "\n",
    "print('Yes :',att_total['Yes'])\n",
    "print('No :',att_total['No'])\n",
    "print('Attrition Rate:',att_rate,'%')"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 11,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Attrition\n",
       "No     37.561233\n",
       "Yes    33.607595\n",
       "dtype: float64"
      ]
     },
     "execution_count": 11,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "att_by_age = data[['Attrition','Age','PerformanceRating']]\n",
    "att_total_age = att_by_age.groupby(['Attrition']).sum()\n",
    "att_by_age_corr = att_total_age['Age']/att_total\n",
    "att_by_age_corr"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>PerformanceRating</th>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Age</th>\n",
       "      <th></th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>18</th>\n",
       "      <td>3.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>19</th>\n",
       "      <td>3.444444</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>20</th>\n",
       "      <td>3.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>21</th>\n",
       "      <td>3.153846</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>22</th>\n",
       "      <td>3.250000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>23</th>\n",
       "      <td>3.214286</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>24</th>\n",
       "      <td>3.115385</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>25</th>\n",
       "      <td>3.115385</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>26</th>\n",
       "      <td>3.256410</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>27</th>\n",
       "      <td>3.104167</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>28</th>\n",
       "      <td>3.125000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>29</th>\n",
       "      <td>3.088235</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>30</th>\n",
       "      <td>3.216667</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>31</th>\n",
       "      <td>3.173913</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>32</th>\n",
       "      <td>3.163934</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>33</th>\n",
       "      <td>3.155172</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>34</th>\n",
       "      <td>3.155844</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>35</th>\n",
       "      <td>3.115385</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>36</th>\n",
       "      <td>3.188406</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>37</th>\n",
       "      <td>3.100000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>38</th>\n",
       "      <td>3.137931</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>39</th>\n",
       "      <td>3.166667</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>40</th>\n",
       "      <td>3.175439</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>41</th>\n",
       "      <td>3.175000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>42</th>\n",
       "      <td>3.130435</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>43</th>\n",
       "      <td>3.187500</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>44</th>\n",
       "      <td>3.121212</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>45</th>\n",
       "      <td>3.170732</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>46</th>\n",
       "      <td>3.212121</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>47</th>\n",
       "      <td>3.083333</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>48</th>\n",
       "      <td>3.157895</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>49</th>\n",
       "      <td>3.166667</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>50</th>\n",
       "      <td>3.066667</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>51</th>\n",
       "      <td>3.210526</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>52</th>\n",
       "      <td>3.111111</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>53</th>\n",
       "      <td>3.157895</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>54</th>\n",
       "      <td>3.111111</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>55</th>\n",
       "      <td>3.136364</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>56</th>\n",
       "      <td>3.142857</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>57</th>\n",
       "      <td>3.250000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>58</th>\n",
       "      <td>3.285714</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>59</th>\n",
       "      <td>3.200000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>60</th>\n",
       "      <td>3.200000</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "     PerformanceRating\n",
       "Age                   \n",
       "18            3.000000\n",
       "19            3.444444\n",
       "20            3.000000\n",
       "21            3.153846\n",
       "22            3.250000\n",
       "23            3.214286\n",
       "24            3.115385\n",
       "25            3.115385\n",
       "26            3.256410\n",
       "27            3.104167\n",
       "28            3.125000\n",
       "29            3.088235\n",
       "30            3.216667\n",
       "31            3.173913\n",
       "32            3.163934\n",
       "33            3.155172\n",
       "34            3.155844\n",
       "35            3.115385\n",
       "36            3.188406\n",
       "37            3.100000\n",
       "38            3.137931\n",
       "39            3.166667\n",
       "40            3.175439\n",
       "41            3.175000\n",
       "42            3.130435\n",
       "43            3.187500\n",
       "44            3.121212\n",
       "45            3.170732\n",
       "46            3.212121\n",
       "47            3.083333\n",
       "48            3.157895\n",
       "49            3.166667\n",
       "50            3.066667\n",
       "51            3.210526\n",
       "52            3.111111\n",
       "53            3.157895\n",
       "54            3.111111\n",
       "55            3.136364\n",
       "56            3.142857\n",
       "57            3.250000\n",
       "58            3.285714\n",
       "59            3.200000\n",
       "60            3.200000"
      ]
     },
     "execution_count": 12,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "att_age_perf = att_by_age.groupby(['Age']).count()\n",
    "att_total_perf = att_by_age.groupby(['Age']).sum()\n",
    "\n",
    "att_perf_by_age = att_total_perf/att_age_perf\n",
    "att_perf_by_age[['PerformanceRating']]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 13,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th>MaritalStatus</th>\n",
       "      <th>Divorced</th>\n",
       "      <th>Married</th>\n",
       "      <th>Single</th>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Attrition</th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>No</th>\n",
       "      <td>294</td>\n",
       "      <td>589</td>\n",
       "      <td>350</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Yes</th>\n",
       "      <td>33</td>\n",
       "      <td>84</td>\n",
       "      <td>120</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "MaritalStatus  Divorced  Married  Single\n",
       "Attrition                               \n",
       "No                  294      589     350\n",
       "Yes                  33       84     120"
      ]
     },
     "execution_count": 13,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "att_by_marital = data[['Attrition','MaritalStatus','PerformanceRating']]\n",
    "att_total_marital = pd.crosstab(index=data['Attrition'], columns=data['MaritalStatus'])\n",
    "att_total_marital"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 14,
   "metadata": {},
   "outputs": [],
   "source": [
    "percent_att_marital_single=(att_total_marital['Single']['Yes']/(att_total_marital['Single']['Yes']+att_total_marital['Single']['No']))*100\n",
    "percent_att_marital_married=(att_total_marital['Married']['Yes']/(att_total_marital['Married']['Yes']+att_total_marital['Married']['No']))*100\n",
    "percent_att_marital_divorced=(att_total_marital['Divorced']['Yes']/(att_total_marital['Divorced']['Yes']+att_total_marital['Divorced']['No']))*100\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 15,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>PerformanceRating</th>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>MaritalStatus</th>\n",
       "      <th></th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>Divorced</th>\n",
       "      <td>3.146789</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Married</th>\n",
       "      <td>3.157504</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Single</th>\n",
       "      <td>3.153191</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "               PerformanceRating\n",
       "MaritalStatus                   \n",
       "Divorced                3.146789\n",
       "Married                 3.157504\n",
       "Single                  3.153191"
      ]
     },
     "execution_count": 15,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "att_marital_perf = att_by_marital.groupby(['MaritalStatus']).count()\n",
    "att_total_perf= att_by_marital.groupby(['MaritalStatus']).sum()\n",
    "att_total_perf= att_by_marital.groupby(['MaritalStatus']).sum()\n",
    "\n",
    "att_marital_perf_rate = att_total_perf/att_marital_perf\n",
    "att_marital_perf_rate[['PerformanceRating']]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 16,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>DistanceFromHome</th>\n",
       "      <th>PerformanceRating</th>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Attrition</th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>No</th>\n",
       "      <td>8.915653</td>\n",
       "      <td>3.153285</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Yes</th>\n",
       "      <td>10.632911</td>\n",
       "      <td>3.156118</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "           DistanceFromHome  PerformanceRating\n",
       "Attrition                                     \n",
       "No                 8.915653           3.153285\n",
       "Yes               10.632911           3.156118"
      ]
     },
     "execution_count": 16,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "att_by_dist = data[['Attrition','DistanceFromHome','PerformanceRating']]\n",
    "att_sum_dist = att_by_dist.groupby(['Attrition']).sum()\n",
    "att_total_dist = att_by_dist.groupby(['Attrition']).count()\n",
    "att_sum_dist/att_total_dist"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 17,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Education</th>\n",
       "      <th>PerformanceRating</th>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Attrition</th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>No</th>\n",
       "      <td>2.927007</td>\n",
       "      <td>3.153285</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Yes</th>\n",
       "      <td>2.839662</td>\n",
       "      <td>3.156118</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "           Education  PerformanceRating\n",
       "Attrition                              \n",
       "No          2.927007           3.153285\n",
       "Yes         2.839662           3.156118"
      ]
     },
     "execution_count": 17,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "att_by_edu = data[['Attrition','Education','PerformanceRating']]\n",
    "att_sum_edu = att_by_edu.groupby(['Attrition']).sum()\n",
    "att_total_edu = att_by_edu.groupby(['Attrition']).count()\n",
    "att_sum_edu/att_total_edu"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 18,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>RelationshipSatisfaction</th>\n",
       "      <th>PerformanceRating</th>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Attrition</th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>No</th>\n",
       "      <td>2.733982</td>\n",
       "      <td>3.153285</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Yes</th>\n",
       "      <td>2.599156</td>\n",
       "      <td>3.156118</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "           RelationshipSatisfaction  PerformanceRating\n",
       "Attrition                                             \n",
       "No                         2.733982           3.153285\n",
       "Yes                        2.599156           3.156118"
      ]
     },
     "execution_count": 18,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "att_by_rel = data[['Attrition','RelationshipSatisfaction','PerformanceRating']]\n",
    "att_sum_rel = att_by_rel.groupby(['Attrition']).sum()\n",
    "att_total_rel = att_by_rel.groupby(['Attrition']).count()\n",
    "att_sum_rel/att_total_rel"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 19,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>StockOptionLevel</th>\n",
       "      <th>PerformanceRating</th>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Attrition</th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>No</th>\n",
       "      <td>0.845093</td>\n",
       "      <td>3.153285</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Yes</th>\n",
       "      <td>0.527426</td>\n",
       "      <td>3.156118</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "           StockOptionLevel  PerformanceRating\n",
       "Attrition                                     \n",
       "No                 0.845093           3.153285\n",
       "Yes                0.527426           3.156118"
      ]
     },
     "execution_count": 19,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "att_by_so = data[['Attrition','StockOptionLevel','PerformanceRating']]\n",
    "att_sum_so = att_by_so.groupby(['Attrition']).sum()\n",
    "att_total_so = att_by_so.groupby(['Attrition']).count()\n",
    "att_sum_so/att_total_so"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 20,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>TotalWorkingYears</th>\n",
       "      <th>PerformanceRating</th>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Attrition</th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>No</th>\n",
       "      <td>11.862936</td>\n",
       "      <td>3.153285</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Yes</th>\n",
       "      <td>8.244726</td>\n",
       "      <td>3.156118</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "           TotalWorkingYears  PerformanceRating\n",
       "Attrition                                      \n",
       "No                 11.862936           3.153285\n",
       "Yes                 8.244726           3.156118"
      ]
     },
     "execution_count": 20,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "att_by_twy = data[['Attrition','TotalWorkingYears','PerformanceRating']]\n",
    "att_sum_twy = att_by_twy.groupby(['Attrition']).sum()\n",
    "att_total_twy = att_by_twy.groupby(['Attrition']).count()\n",
    "att_sum_twy/att_total_twy"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 21,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>WorkLifeBalance</th>\n",
       "      <th>PerformanceRating</th>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Attrition</th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>No</th>\n",
       "      <td>2.781022</td>\n",
       "      <td>3.153285</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Yes</th>\n",
       "      <td>2.658228</td>\n",
       "      <td>3.156118</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "           WorkLifeBalance  PerformanceRating\n",
       "Attrition                                    \n",
       "No                2.781022           3.153285\n",
       "Yes               2.658228           3.156118"
      ]
     },
     "execution_count": 21,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "att_by_wlb = data[['Attrition','WorkLifeBalance','PerformanceRating']]\n",
    "att_sum_wlb = att_by_wlb.groupby(['Attrition']).sum()\n",
    "att_total_wlb = att_by_wlb.groupby(['Attrition']).count()\n",
    "att_sum_wlb/att_total_wlb"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 22,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>YearsAtCompany</th>\n",
       "      <th>PerformanceRating</th>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Attrition</th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>No</th>\n",
       "      <td>7.369019</td>\n",
       "      <td>3.153285</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Yes</th>\n",
       "      <td>5.130802</td>\n",
       "      <td>3.156118</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "           YearsAtCompany  PerformanceRating\n",
       "Attrition                                   \n",
       "No               7.369019           3.153285\n",
       "Yes              5.130802           3.156118"
      ]
     },
     "execution_count": 22,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "att_by_yac = data[['Attrition','YearsAtCompany','PerformanceRating']]\n",
    "att_sum_yac = att_by_yac.groupby(['Attrition']).sum()\n",
    "att_total_yac = att_by_yac.groupby(['Attrition']).count()\n",
    "att_sum_yac/att_total_yac"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 76,
   "metadata": {},
   "outputs": [],
   "source": [
    "age_to_perf_1 = data[data['Age']<data.describe()['Age']['50%']]\n",
    "age_to_perf_2 = data[data['Age']>data.describe()['Age']['50%']]\n",
    "avg_age_to_perf_1 = age_to_perf_1['PerformanceRating'].mean()\n",
    "avg_age_to_perf_2 = age_to_perf_2['PerformanceRating'].mean()\n",
    "\n",
    "dist_to_perf_1 = data[data['DistanceFromHome']<data.describe()['DistanceFromHome']['50%']]\n",
    "dist_to_perf_2 = data[data['DistanceFromHome']>data.describe()['DistanceFromHome']['50%']]\n",
    "avg_dist_to_perf_1 = dist_to_perf_1['PerformanceRating'].mean()\n",
    "avg_dist_to_perf_1 = dist_to_perf_2['PerformanceRating'].mean()\n",
    "\n",
    "edu_to_perf_1 = data[data['Education']<data.describe()['Education']['50%']]\n",
    "edu_to_perf_2 = data[data['Education']>data.describe()['Education']['50%']]\n",
    "avg_edu_to_perf_1 = edu_to_perf_1['PerformanceRating'].mean()\n",
    "avg_edu_to_perf_2 = edu_to_perf_2['PerformanceRating'].mean()\n",
    "\n",
    "rel_to_perf_1 = data[data['RelationshipSatisfaction']<data.describe()['RelationshipSatisfaction']['50%']]\n",
    "rel_to_perf_2 = data[data['RelationshipSatisfaction']>data.describe()['RelationshipSatisfaction']['50%']]\n",
    "avg_rel_to_perf_1 = rel_to_perf_1['PerformanceRating'].mean()\n",
    "avg_rel_to_perf_2 = rel_to_perf_2['PerformanceRating'].mean()\n",
    "\n",
    "so_to_perf_1 = data[data['StockOptionLevel']<data.describe()['StockOptionLevel']['50%']]\n",
    "so_to_perf_2 = data[data['StockOptionLevel']>data.describe()['StockOptionLevel']['50%']]\n",
    "avg_so_to_perf_1 = so_to_perf_1['PerformanceRating'].mean()\n",
    "avg_so_to_perf_2 = so_to_perf_2['PerformanceRating'].mean()\n",
    "\n",
    "twy_to_perf_1 = data[data['TotalWorkingYears']<data.describe()['TotalWorkingYears']['50%']]\n",
    "twy_to_perf_2 = data[data['TotalWorkingYears']>data.describe()['TotalWorkingYears']['50%']]\n",
    "avg_twy_to_perf_1 = twy_to_perf_1['PerformanceRating'].mean()\n",
    "avg_twy_to_perf_2 = twy_to_perf_2['PerformanceRating'].mean()\n",
    "\n",
    "wlb_to_perf_1 = data[data['WorkLifeBalance']<data.describe()['WorkLifeBalance']['50%']]\n",
    "wlb_to_perf_2 = data[data['WorkLifeBalance']>data.describe()['WorkLifeBalance']['50%']]\n",
    "avg_wlb_to_perf_1 = wlb_to_perf_1['PerformanceRating'].mean()\n",
    "avg_wlb_to_perf_2 = wlb_to_perf_2['PerformanceRating'].mean()\n",
    "\n",
    "yac_to_perf_1 = data[data['YearsAtCompany']<data.describe()['YearsAtCompany']['50%']]\n",
    "yac_to_perf_2 = data[data['YearsAtCompany']>data.describe()['YearsAtCompany']['50%']]\n",
    "yac_edu_to_perf_1 = yac_to_perf_1['PerformanceRating'].mean()\n",
    "yac_edu_to_perf_2 = yac_to_perf_2['PerformanceRating'].mean()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 96,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 216x216 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 216x216 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "fig = plt.subplots(figsize=(3,3))\n",
    "ax = sns.barplot(x=\"Attrition\", y=\"Age\", data=data)\n",
    "fig2 = plt.subplots(figsize=(3,3))\n",
    "ax2 = sns.barplot(x=\"PerformanceRating\", y=\"Age\", data=data)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 97,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "iVBORw0KGgoAAAANSUhEUgAAANcAAADQCAYAAACUePQNAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAALEgAACxIB0t1+/AAAADh0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uMy4yLjIsIGh0dHA6Ly9tYXRwbG90bGliLm9yZy+WH4yJAAAP+UlEQVR4nO3debRdZX3G8e9DgCYyLMQgUKaAMpQig4bWwqqlgLMyOIApKuJAdSGDA4ilBcTVtSyCU61aWlGmaougBUsZtaEuFUgwMk+CTJKSyLKEMIQkT//Y+5bLJfeefU/Oe/Y9J89nrbPO2cPZ7y938ePd593v/m3ZJiJ6b622A4gYVkmuiEKSXBGFJLkiCklyRRSS5IooZO22A2hi5syZnjVrVtthRLzA/PnzF9veZFXbiiWXpLOBtwCP2t6lXvd54K3AMuBXwBG2f9fpWLNmzWLevHmlQo3omqT7x9tW8rTw28Abxqy7CtjF9q7AXcCnC7Yf0apiyWX7WuCxMeuutL28Xvw5sGWp9iPa1uaAxvuB/xxvo6QjJc2TNG/RokV9DCuiN1pJLkknAcuBC8bbx/ZZtmfbnr3JJqv8vRgxpfV9tFDS4VQDHft5iGYNn3DCCSxcuJDNNtuM008/ve1wYgroa3JJegPwKeDPbD/Zz7ZLW7hwIQ8//HDbYcQUUuy0UNJ3gJ8BO0p6SNIHgK8CGwBXSVog6Rul2o9oW7Gey/acVaz+Zqn2IqaaTH+KKCTJFVFIkiuikCRXRCFJrohCklwRhSS5IgpJckUUkuSKKCTJFVFIkiuikCRXRCEDUf2pk1cdf27bIbDB4iVMAx5YvKTVeOZ//r2ttR3Pl54ropAkV0QhSa6IQpJcEYWUvM3/bEmPSrpl1LqNJV0l6e76/cWl2o9oW78r7p4IXGN7e+CaejliKPW14i5wIHBO/fkc4KBS7Ue0rd+/uTa1/QhA/f7SPrcf0TdTdkAj5axj0PU7uf5H0uYA9fuj4+2YctYx6PqdXJcAh9efDwf+vc/tR/RNvyvufg54raS7gdfWyxFDaVITdyWtZ3tpk33HqbgLsN9k2owYVI16Lkl7SboNuL1e3k3S14pGFjHgmp4WfhF4PfBbANu/BF5TKqhBtHLd9Vjxexuyct312g4lpojGp4W2H5Q0etWK3oczuJZu/7q2Qxgqw/C8s6bJ9aCkvQBLWhc4hvoUMaKEYXjeWdPTwg8DRwFbAA8Bu9fLETGORj2X7cXAYYVjiRgqjZJL0rbA0cCs0d+xfUCZsCIGX9PfXD+geirkpcDKcuFEDI+myfW07a8UjSRiyDRNri9LOgW4EnhmZKXtG4tEFa164LRXtB0Cyx/bGFib5Y/d32o8W598c9ffbZpcrwDeA+zLc6eFrpcjYhWaJtfBwHa2l5UMJmKYNL3O9Utgo5KBRAybpj3XpsAdkm7g+b+5MhQfMY6myXVK0SgihlDTGRpzJW0K7Fmvut72uLfoR0Tz+7kOAa4H3gkcAlwn6R0lA4s128zpK9l0xnJmTh/cOQtNTwtPAvYc6a0kbQJcDXyvm0YlfQz4INVw/s3AEbaf7uZYMZw+uevv2g5htTUdLVxrzGngbyfx3eeRtAXVLSuzbe8CTAPe1c2xIqaypj3X5ZKuAL5TLx8KXLaa7c6Q9CzwIuA3q3GsiCmp6YDG8ZLeDuwNCDjL9ve7adD2w5LOAB4AngKutH1lN8eKmMomc5v/RcBFq9tg/WSTA4Ftgd8BF0p6t+3zx+x3JHAkwNZbb726zUb03YS/myQtkfT4Kl5LJD3eZZv7A/fZXmT7WeBiYK+xO6Xibgy6CXsu2xuMfJb0C9t79KDNB4BXS3oR1WnhfsC8Hhw3YkqZzIife9Gg7euohvBvpBqGXws4qxfHjphKJlVxt1dsn0KmVMWQmzC5JL1t1OJGY5axfXGRqCKGQKee662jPs8ds2yqwYiIWIVOAxpH9CuQiGHTtLTaRsB7eWFptWPKhBUx+JoOaFwG/JxqdG9wpylH9FHT5Jpu++NFI4kYMk2vc50n6UOSNpe08ciraGQRA65pz7UM+DzVfV0jF5MNbFciqIhh0DS5Pg68vH4gQ0Q00PS08FbgyZKBRAybpj3XCmCBpB/z/NJqGYqPGMdknnLyg5KBRAybpncin1M/rnWHetWd9b1YETGOpjM09gHOAX5NdZv/VpIOt31tudAiBlvT08IzgdfZvhNA0g5UxWpeVSqwiEHXdLRwnZHEArB9F7BOmZAihkPTnmu+pG8C59XLhwHzy4QUMRyaJteHgaOoinkKuBb4WqmgIoZBx+SStBYwv66O+4VeNFrfwvLPwC5U06jeb/tnvTh2xFTR8TeX7ZXALyX1snjgl4HLbe8E7Abc3sNjR0wJTU8LNwdulXQ9sHRkZTcPv5O0IfAa4H31MZZRTQyOGCqdCtSsbXs58JketrkdsAj4lqTdqAZGjrW9dPROqbgbg67TaeH1UD38DniH7bmjX122uTbwSuDrdZHRpcCJY3dKxd0YdJ2SS6M+792jNh8CHqqLg0JVIPSVPTp2xJTRKbl6UmX3eQe0FwIPStqxXrUfcFuv24loW6cBjZ0k3UTVg72s/ky9bNu7dtnu0cAF9WTge4GUcIuh0ym5/qBEo7YXALNLHDtiquhUFPT+kc+StgG2t321pBmdvhuxpms0cVfSh6gGHv6xXrUluXkyYkJNZ8UfRTVa+DiA7buBl5YKKmIYNE2uZ+qZFEB1cZkCI4kRw6Rpcs2V9FfADEmvBS4ELi0XVsTga5pcJ1JNWboZ+Euq2vF/XSqoiGHQdMRvBnC27X8CkDStXpdahhHjaNpzXUOVTCNmAFf3PpyI4dE0uabbfmJkof78ojIhRQyHpsm1VNL/T66V9CrgqTIhRQyHpr+5jgMulPSbenlz4NAyIUUMh6YVd2+QtBOwI9Wk3TtScTdiYpOZH7gnzz0TeQ9J2D63SFQRQ6BpOevzgJcBC6ieeALVDI0kV8Q4mvZcs4GdbWfKU0RDTUcLbwE2KxlIxLBp2nPNBG6rS6uNfvjdpEurRawpmibXqb1uuJ5CNQ942PZben38iLY1HYrvtozaRI6lqrS7YYFjR7Su6Z3Ir5Z0g6QnJC2TtELS4902KmlL4M1U9eIjhlLTAY2vAnOAu6km7X6wXtetLwEnACtX4xgRU1rT5ML2PcA02ytsfwvYp5sGJb0FeNT2hM/3knSkpHmS5i1atKibpiJa1TS5nqxrDC6QdLqkjwHrddnm3sABkn4NfBfYV9L5Y3dKOesYdE2T6z31vh+lqu2+FfC2bhq0/WnbW9qeBbwL+JHtd3dzrIiprGlyHWT7aduP2/6M7Y8DGT6PmEDT5Dp8Fevet7qN2/6vXOOKYdXp+VxzgL8AtpV0yahNGwK/LRlYxKDrdBH5p8AjVNOfzhy1fglw0yq/ERFAs1rx90vaH3jK9kpJOwA7UZVZi4hxNP3NdS0wXdIWVJWgjgC+XSqoiGHQNLlk+0mq4fe/t30wsHO5sCIGX+PkkvQnwGHAf9Tr8gihiAk0Ta7jgE8D37d9q6TtgB+XCyti8E3mlpO5o5bvBY4pFVTEMOh0netLto+TdCmreGRQ7kSOGF+nnuu8+v2M0oFEDJtO17nm1+9zJW1Sf879HxENTDigocqpkhYDdwB3SVok6eT+hBcxuDqNFh5Hdf/VnrZfYvvFwB8De9f3dEXEODol13uBObbvG1lRjxS+u94WEePolFzr2F48dmX9u2udMiFFDIdOybWsy20Ra7xOQ/G7jVNCTcD0AvFEDI1OQ/HT+hVIxLBpXFqtVyRtJenHkm6XdKukY/sdQ0Q/tDGzfTnwCds3StoAmC/pKtu3tRBLRDF977lsP2L7xvrzEqp68Vv0O46I0vqeXKNJmgXsAVy3im2puBsDrbXkkrQ+cBFwnO0XjEim4m4MulaSS9I6VIl1ge2L24ghorQ2RgsFfBO43fYX+t1+RL+00XPtTVV7fl9JC+rXm1qII6Kovg/F2/4J1QyPiKHW6mhhxDBLckUUkuSKKCTJFVFIkiuikCRXRCFJrohCklwRhSS5IgpJckUUkuSKKCTJFVFIkiuikCRXRCFJrohCklwRhSS5Igppq0DNGyTdKekeSSe2EUNEaW0UqJkG/APwRmBnYI6knfsdR0RpbfRcfwTcY/te28uA7wIHthBHRFFtJNcWwIOjlh8i5axjCLXxIIZVVX7yC3aSjgSOrBefkHRn0ah6Yybwgidx9pPOOLzN5nut9b8np3QsVLbNeBvaSK6HgK1GLW8J/GbsTrbPAs7qV1C9IGme7dltxzEsBv3v2cZp4Q3A9pK2lbQu8C7gkhbiiCiqjaKgyyV9FLgCmAacbfvWfscRUVobp4XYvgy4rI22Cxuo09gBMNB/T9kvGEuIiB7I9KeIQpJck6DKTyS9cdS6QyRd3mZcg06SJZ05avmTkk5tMaSeSHJNgqtz6A8DX5A0XdJ6wN8CR7Ub2cB7BnibpJltB9JLSa5Jsn0LcCnwKeAU4HzgJEk3SPqFpAMBJP2hpOvr54/dJGn7FsOe6pZTDV58bOwGSdtIuqb+G14jaev+h9edDGh0oe6xbgSWAT8EbrV9vqSNgOupHqL+OeDnti+or+dNs/1Ua0FPYZKeAH4fuAnYDfgQsL7tUyVdCnzP9jmS3g8cYPugFsNtLMnVJUmnAU8AhwDTqf7vC7Ax8HqqBDsJOBe42PbdbcQ5CCQ9YXv9+m/6LPAUzyXXYmBz28/Wz9J+xPZAnD62cp1rSKysXwLebnvs3MfbJV0HvBm4QtIHbf+o30EOmC9RnRF8a4J9BqY3yG+u1XcFcHT9IHUk7VG/bwfca/srVNO7dm0vxMFg+zHg34APjFr9U6opcgCHAT/pd1zdSnKtvs8C6wA3SbqlXgY4FLhF0gJgJ6rTw+jsTKrZ8COOAY6QdBPVg+qPbSWqLuQ3V0Qh6bkiCklyRRSS5IooJMkVUUiSK6KQJNeAkHRwPXt8p3p5d0lvGrV9H0l7TfD9A0YKsEo6aHStSEmnSdq/ZPxroiTX4JhDdQF15ILq7sCbRm3fB1hlckla2/Yltj9XrzqIqiArALZPtn11zyNew+U61wCQtD5wJ/DnPDfb4x5gBvAw8B2qGeUrgEXA0VSzHB6jmuN4I3AzMBv4F6rJxv9bv94O/A3wQ9vfk7QfcAbV1LgbgI/YfkbSr4FzgLdSXTR/p+07Sv/bB1l6rsFwEHC57buoEmYX4GTgX23vbvvvgG8AX6yX/7v+3g7A/rY/MXIg2z+lStDj631/NbJN0nTg28Chtl9BlWAfGRXHYtuvBL4OfLLQv3VoJLkGwxyqst/U73Mafu9C2ysm0c6OwH11EkPVU71m1PaL6/f5wKxJHHeNlFnxU5yklwD7ArtIMlU5OlPdqNnJ0sk212H7M/X7CvLfTkfpuaa+dwDn2t7G9izbWwH3AVsDG4zab8mY5YmMt+8dwCxJL6+X3wPM7S7sSHJNfXOA749ZdxGwGbBzXUbgUKrSAwfXy3/a4ZjfBY6vyxK8bGSl7aeBI4ALJd1Mdb/aN3r1D1nTZLQwopD0XBGFJLkiCklyRRSS5IooJMkVUUiSK6KQJFdEIUmuiEL+D7sA3IjeAYn0AAAAAElFTkSuQmCC\n",
      "text/plain": [
       "<Figure size 216x216 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 216x216 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "fig = plt.subplots(figsize=(3,3))\n",
    "ax = sns.barplot(x=\"Attrition\", y=\"DistanceFromHome\", data=data)\n",
    "fig2 = plt.subplots(figsize=(3,3))\n",
    "ax2 = sns.barplot(x=\"PerformanceRating\", y=\"DistanceFromHome\", data=data)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 98,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 216x216 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 216x216 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "fig = plt.subplots(figsize=(3,3))\n",
    "ax = sns.barplot(x=\"Attrition\", y=\"Education\", data=data)\n",
    "fig2 = plt.subplots(figsize=(3,3))\n",
    "ax2 = sns.barplot(x=\"PerformanceRating\", y=\"Education\", data=data)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 99,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 216x216 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    },
    {
     "data": {
      "image/png": "iVBORw0KGgoAAAANSUhEUgAAANoAAADQCAYAAABhhn+9AAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAALEgAACxIB0t1+/AAAADh0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uMy4yLjIsIGh0dHA6Ly9tYXRwbG90bGliLm9yZy+WH4yJAAATdUlEQVR4nO3debRdZXnH8e+PJDTMQxMFgSQySZFJiSDqkmE5AFKwQBFKFdGWyiBQilS7BBVcKKBUMQpGZCxKRZGpUKERCCpTAkkIBJQGUFBmCQlQTMLTP973wsn13Lvfm3v3OTmb32ets87e++x3n+fe3Cd7evfzKiIws3qt1O0AzF4PnGhmHeBEM+sAJ5pZBzjRzDrAiWbWAaO7HcBQjRs3LiZNmtTtMMzamjlz5tMRMb7/8p5LtEmTJjFjxoxuh2HWlqRH2i33oaNZBzjRzDrAiWbWAU40sw7ouYshveCEE07g8ccfZ7311uP000/vdji2AnCi1eDxxx/nscce63YYjdGE/7icaLbCa8J/XD5HM+sAJ5pZBzTy0HH7z1zU1e9f4+mFjAJ++/TCrscy84yPdfX7LfEezawDnGhmHeBEM+uARp6jddsrK6+2zLuZE60GL2z2gW6HMGJ+e/LW3Q6BJc+uC4xmybOPdDWeCSfds9xtfeho1gFONLMOcKKZdUBtiSZpI0k3Spon6V5Jx7RZZxdJCyTNyq+T6orHrJvqvBiyBPiXiLhL0hrATEk3RMR9/da7JSL2qjEOs66rbY8WEX+IiLvy9EJgHrBBXd9ntiLryDmapEnA24Db23y8k6TZkq6T9NYB2h8maYakGU899VSNkZrVo/jQUdIGwMTWNhExvaDd6sBPgGMj4vl+H98FTIyIRZL2BK4ANuu/jYiYCkwFmDx5sseZsp5TlGiSTgM+AtwHLM2LAxg00SSNISXZJRFxef/PWxMvIq6V9B1J4yLi6cL47XVg3NhXgCX5vTeV7tE+DLwlIl4u3bAkAd8H5kXEmQOssx7wRESEpB1Ih7LPlH6HvT4cv81z3Q5h2EoTbT4wBihONODdwEeBeyTNysv+DZgAEBHnAPsDh0taArwEHBgegtQaqDTRXgRmSZpGS7JFxNEDNYiIXwAabKMRMQWYUhiDWc8qTbSr8svMlkNRokXEhZJWBjbPix6IiMX1hWXWLKVXHXcBLgQeJh0ObiTpkJLL+2ZWfuj4deADEfEAgKTNgR8C29cVmFmTlPYMGdOXZAAR8WvSVUgzK1C6R5sh6fvAxXn+YGBmPSGZNU9poh0OHAkcTTpHmw58p66gzJqm9Krjy8CZ+WVmQzRookn6UUQcIOkeUt/GZUTENrVFZtYgVXu0vqei/WCm2TAMetUxIv6QJ4+IiEdaX8AR9Ydn1gyll/ff32bZHiMZiFmTVZ2jHU7ac20iaU7LR2sAv6ozMLMmqTpH+wFwHfAV4LMtyxdGxLO1RWXWMFXnaAsi4mHgm8CzLedniyXt2IkAzZqg9BztbGBRy/wLeZmZFShNNLU++RwRr+ABMsyKlSbafElHSxqTX8eQyhuYWYHSRPsU8C7gMeBRYEfgsLqCMmua0r6OTwIHDmXDkjYCLgLWA14BpkbEN/utI9KFlj1JdUk+3lfd2KxJSp+wHgt8EngrMLZveUR8YpBmJbX39yAVTN2MtJc8O7+bNUrpoePFpD3TB4GbgQ2BhYM1KKy9vw9wUSS3AWtLWn8I8Zv1hNJE2zQiTgReiIgLgQ8BxWOcDlJ7fwPgdy3zj9JmIAzX3rdeV5pofRWvnpO0FbAWMKmkYUXt/XZ1H9s9jjM1IiZHxOTx48cXhmy24ii9FzZV0jrA50n1HVcHTqxqVFV7n7QH26hlfkPg94UxmfWMQfdoLaN0zouIP0bE9IjYOCLeEBHfrWhbWXuflLQfU/JOYEHLozlmjVG1RzuUdPn9W8Dbh7jtktr715Iu7T9Iurx/6BC/w6wnVCXaPEkPA+P7PSYjIAYrZVBYez9IRX/MGm3QRIuIg/LQSj8D9u5MSGbNU3kxJCIeB7btm88XRTaKiDkDtzKzVkWX9yXdJGlNSesCs4HzJbn0nFmh0vtoa+V7YPsC50fE9sD76gvLrFlKE2107hp1AHBNjfGYNVJpop1MuiDyYETcKWlj4Df1hWXWLKWPyVwGXNYyPx/Yr66gzJqmqtzcCRFxuqRv0b4P4oBjWJvZaypvWOf3GXUHYtZkVTesr86TL+bDx1dJ+tvaojJrmNKLIZ8rXGZmbVSdo+1B6vS7gaSzWj5ak1SqwMwKVJ2j/Z50frY3yw6luxD457qCMmuaqnO02cBsST+IiMWDrWtmAyt9wnqSpK8AW7JsFayNa4nKrGFKL4acTyoFtwTYlVSv8eK6gjJrmtJEWyUippFq8D8SEV8EdqsvLLNmKT10/D9JKwG/kXQUqTT4G+oLy6xZSvdoxwKrAkcD25NqgRxSV1BmTVPaqfjOPLlI0nHAc63DOLUj6TxgL+DJiNiqzee7AFcCD+VFl0fEyaWBm/WSqnJzJ0naIk//haQbgf8FnpBU9eDnBcDuFevcEhHb5ZeTzBqr6tDxI8ADebrvUHE8sDNw6mANI2I64HGuzahOtD+1HCJ+ELg0IpZGxDxGZsTPnSTNlnSdpLcOtJJr71uvq0q0lyVtJWk86f7Z9S2frTrM774LmBgR25IKtF4x0IquvW+9rirRjgF+DNwP/HtEPAQgaU/g7uF8cUQ8HxGL8vS1wBhJ44azTbMVVVVfx9uBLdosv5ZUznu55cKsT0RESNqBlPTPDGebZiuq0hE//xL4AvAeUkmDXwAnR8SAiSHph8AuwDhJj+b2Y+DVuvv7A4dLWgK8BBxYdcvArFeVXtC4FJjOawV5Dgb+k0FqO0bEQYNtMCKmAFMKv9+sp5Um2roRcUrL/JclfbiOgMyaqLQL1o2SDpS0Un4dAPxXnYGZNUlpov0T8APgZeBPpEPJ4yQtlNR/uFwz66e0r+MadQdi1mRVxXm2iIj7JbUd7TMi7qonLLNmqdqjHQccBny9zWeBH/40K1J1w/qw/L5rZ8Ixa6bijsGS3gVMam0TERfVEJNZ45T2DLkY2ASYBSzNi4NUpMfMKpTu0SYDW7qLlNnyKb2PNhdYr85AzJqs6vL+1aRDxDWA+yTdQbppDUBE7F1veGbNUHXo+LWORGHWcFWX928GkLQa8FJEvCJpc9Izatd1ID6zRig9R5sOjJW0ATANOJRU5crMCpQmmiLiRWBf4FsR8TfAgMV0zGxZxYkmaSfSA599j8eMqicks+YpTbRjSEPp/jQi7pW0MXBjfWGZNUtRokXE9IjYOyJOy/PzI+LowdpIOk/Sk5LmDvC5JJ0l6UFJcwZ6QsCsCUq7YG0OHM+f93UcrPf+BaSaIAN109oD2Cy/diSNv7ZjSTxmvaa0C9ZlwDnAubzW13FQETFd0qRBVtkHuCh367pN0tqS1o+IPxTGZNYzShNtSUScPcLfvQHwu5b5R/MyJ5o1TunFkKslHSFpfUnr9r2G+d1qs6xtp2XX3rdeV7pH6xtJ5jMtywIYzmDxjwIbtcxvCPy+3YoRMRWYCjB58mQ/QWA9p7Q4z5tr+O6rgKMkXUq6CLLA52fWVKVXHccAhwPvzYtuAr4bEYsHaVNVEvxaYE/gQeBFUrcus0YqPXQ8m5Qk38nzH83L/mGgBgUlwQM4svD7zXpaaaK9I49j1ufnkmbXEZBZE5VedVwqaZO+mdwFq+h+mpmV79E+Q6q/P590WX4iPqcyK1Z61XGapM2At5AS7f6IeLmimZllVTVDdouIn0vat99Hm0giIi6vMTazxqjao+0M/Bz46zafBeBEMytQVTPkC3ny5L6B4vtIquMmtlkjlV51/EmbZT8eyUDMmqxy2CZSbZC1+p2nrQmMrTMwsyapOkd7C7AXsDbLnqctBP6xrqDMmqbqHO1K4EpJO0XErR2KyaxxSm9Y3y3pSNJh5KuHjBHxiVqiMmuY0oshF5MGufggcDPp2bGFdQVl1jSlibZpRJwIvBARFwIfArauLyyzZilNtL7nzp6TtBWwFqkilpkVKD1HmyppHeBE0pPRqwMn1RaVWcOUdio+N0/ezPDqhJi9LlXdsD5usM8j4syRDcesmar2aGt0JAqzhqu6Yf2l4Wxc0u7AN0kjz5wbEV/t9/kuwJVAX4flyyPi5OF8p9mKqOiqo6TNJU3rG7BC0jaSPl/RZhTwbVKN/S2BgyRt2WbVWyJiu/xyklkjlV7e/x5p2KbFABExBziwos0OwIN55Jk/AZeS6u2bve6UJtqqEXFHv2VLKtoMVFu/v50kzZZ0nSSPImqNVHof7elcBSsAJO1P9WAUJbX17wImRsQiSXsCV5CGcVp2Q9JhwGEAEyZMKAzZbMVRukc7EvgusIWkx4BjgU9VtKmsrR8Rz0fEojx9LTBG0rj+G4qIqRExOSImjx8/vjBksxVH6Yif8yPifcB4YAtSqe/3VDS7E9hM0pslrUw6p7uqdQVJ60lSnt4hx/PMkH4Csx4waKJJWlPS5yRNkfR+Uo38Q0j18g8YrG1ELAGOAn4GzAN+lMe//pSkvr3h/sDcXPX4LODAXCrcrFGqztEuBv4I3Ep6ovoEYGXgwxExq2rj+XDw2n7LzmmZnkIaftes0aoSbeOI2BpA0rnA08CEiPCzaGZDUHWO9uqwTBGxFHjISWY2dFV7tG0lPZ+nBayS50UaeWnNWqMza4iqvo6jOhWIWZOV3kczs2Fwopl1gBPNrAOcaGYd4EQz6wAnmlkHONHMOsCJZtYBTjSzDnCimXWAE82sA5xoZh3gRDPrACeaWQc40cw6oNZEk7S7pAckPSjps20+l6Sz8udzJL29znjMuqW2RCusvb8HqWDqZqQCqWfXFY9ZN9W5Ryupvb8PcFEktwFrS1q/xpjMuqLORCupvV9an9+sp5XW3l8eJbX3S9ZZpvY+sEjSA8OMrRPGkcrzdZW+dki3Qxgp3f99fqHdn+ufmdhuYZ2JVll7v3AdImIqMHWkA6yTpBkRMbnbcTRFr/8+6zx0rKy9n+c/lq8+vhNYEBFVo9SY9Zza9mgRsURSX+39UcB5fbX38+fnkMqF70mq5f8icGhd8Zh1kzymRD0kHZYPeW0E9Prv04lm1gHugmXWAU60ESZprKQ78rjc90r6Urdj6nWSRkm6W9I13Y5leTnRRt7LwG4RsS2wHbB7vqJqy+8Y0mCWPcuJNsJyd7JFeXZMfvlEeDlJ2hD4EHBut2MZDidaDfKhzizgSeCGiLi92zH1sG+QRpp9pduBDIcTrQYRsTQitiP1dNlB0lbdjqkXSdoLeDIiZnY7luFyotUoIp4DbgJ273IoverdwN6SHiY9/bGbpP/obkjLx/fRRpik8cDiiHhO0irA9cBpEdGzV8xWBJJ2AY6PiL26HcvyqLNT8evV+sCF+cHXlYAfOcnMezSzDvA5mlkHONHMOsCJZtYBTjSzDnCimXWAE20ESFoqaZakuZIuk7TqENufkXv6n1FXjHWRdFMukjtb0p2StqtYf21JR7TMv0nSj+uPtLt8eX8ESFoUEavn6UuAmRFxZkG70bnkw/PA+Ih4ufD7RkfEkuFFPTIk3US6kTxD0qHA30XE+wdZfxJwTUS8rrqleY828m4BNpW0mqTz8v/yd0vaB0DSx/Ne72rgeklXAasBt0v6iKSJkqblEunTJE3I7S6QdKakG4HT8vzZkm6UNF/Szvn75km6oC+YvM6M/s/GSXpY0pck3SXpHklb5OWrSzo/L5sjab+8/AOSbs3rXyZp9TY/+63kupx5O9Natt9XPPerwCb5COAMSZMkzW353Vwu6b8l/UbS6S3xflLSr/Me9HuSpozMP1eHRIRfw3wBi/L7aOBK4HDgVODv8/K1gV+TEurjpDJ76/Zvn6evBg7J058ArsjTFwDXAKNa5i8l1cbcB3ge2Jr0n+dMYLu83rr5fRSp3+U2ef5h4NN5+gjg3Dx9GvCNlnjWIdVUnA6slpf9K3BSnr4JmJynjwVObfldrJmnx5EKMAmYBMxt2f6r8/l3Mx9YCxgLPEIqR/imHO+6pMeObgGmdPvffSgvd8EaGavkx2Ig/RF8H/gVqUPs8Xn5WGBCnr4hIp4dYFs7Afvm6YuB01s+uywilrbMXx0RIeke4ImIuAdA0r2kP+BZwAFKBWhHk7qHbQnMye0vz+8zW77zfaTSgABExB9zL/otgV9KAliZtPfqc4mk1UjJ3DdQiYBTJb2X9IjLBsAbB/iZW02LiAX557iPVJB0HHBz3+9M0mXA5gXbWmE40UbGS5Eei3mV0l/kfhHxQL/lOwIvDGHbrSfR/dv1ndO90jLdNz9a0puB44F35IS5gJTw/dsv5bW/BdG+ovQNEXHQADEeDMwmHRZ+m5S0BwPjge0jYnHugT92gPbtfqbWuIpKBK/IfI5Wn58Bn84Jh6S3Fbb7Fa/tUQ4GfjGMGNYkJecCSW8kjd5T5XrgqL4ZSesAtwHvlrRpXraqpGX2KBGxGPg88E5Jf0U6/HsyJ9muvFYqeyGwxhB/jjuAnSWtI2k0sN8Q23edE60+p5DOJ+bkk/1TCtsdDRwqaQ7wUVK9jOUSEbOBu4F7gfOAXxY0+zKwTr5VMRvYNSKeIp0//TDHdRuwRZvvewn4OmkvegkwWdIM0n8Y9+d1niEdgs4tvZ0REY+RznlvB/4HuA9YUNJ2ReHL+9YTJK0eEYvyHu2npMrXP+12XKW8R7Ne8cV8wWku8BBwRZfjGRLv0cw6wHs0sw5wopl1gBPNrAOcaGYd4EQz6wAnmlkH/D9m8A30FuEc9gAAAABJRU5ErkJggg==\n",
      "text/plain": [
       "<Figure size 216x216 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "fig = plt.subplots(figsize=(3,3))\n",
    "ax = sns.barplot(x=\"Attrition\", y=\"RelationshipSatisfaction\", data=data)\n",
    "fig2 = plt.subplots(figsize=(3,3))\n",
    "ax2 = sns.barplot(x=\"PerformanceRating\", y=\"RelationshipSatisfaction\", data=data)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 100,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 216x216 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 216x216 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "fig = plt.subplots(figsize=(3,3))\n",
    "ax = sns.barplot(x=\"Attrition\", y=\"StockOptionLevel\", data=data)\n",
    "fig2 = plt.subplots(figsize=(3,3))\n",
    "ax2 = sns.barplot(x=\"PerformanceRating\", y=\"StockOptionLevel\", data=data)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 101,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 216x216 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 216x216 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "fig = plt.subplots(figsize=(3,3))\n",
    "ax = sns.barplot(x=\"Attrition\", y=\"TotalWorkingYears\", data=data)\n",
    "fig2 = plt.subplots(figsize=(3,3))\n",
    "ax2 = sns.barplot(x=\"PerformanceRating\", y=\"TotalWorkingYears\", data=data)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 102,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 216x216 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 216x216 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "fig = plt.subplots(figsize=(3,3))\n",
    "ax = sns.barplot(x=\"Attrition\", y=\"WorkLifeBalance\", data=data)\n",
    "fig2 = plt.subplots(figsize=(3,3))\n",
    "ax2 = sns.barplot(x=\"PerformanceRating\", y=\"WorkLifeBalance\", data=data)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 103,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 216x216 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 216x216 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "fig = plt.subplots(figsize=(3,3))\n",
    "ax = sns.barplot(x=\"Attrition\", y=\"YearsAtCompany\", data=data)\n",
    "fig2 = plt.subplots(figsize=(3,3))\n",
    "ax2 = sns.barplot(x=\"PerformanceRating\", y=\"YearsAtCompany\", data=data)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.8.3"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 4
}
