---
title: Day 3 Notes
layout: note
---

# Day 3 Notes

## Python Demo and Student Performance - Decision Trees



```python
print("Hello, world!")
```

    Hello, world!



```python
myname = "Josh"
zodiac = "Cancer"
print("Hello, {}, with sign {}".format(myname, zodiac))
```

    Hello, Josh, with sign Cancer



```python
import pandas as pd
```


```python
d = pd.read_csv("/aicamp/student-por.csv", sep=";")
```


```python
d.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>school</th>
      <th>sex</th>
      <th>age</th>
      <th>address</th>
      <th>famsize</th>
      <th>Pstatus</th>
      <th>Medu</th>
      <th>Fedu</th>
      <th>Mjob</th>
      <th>Fjob</th>
      <th>...</th>
      <th>famrel</th>
      <th>freetime</th>
      <th>goout</th>
      <th>Dalc</th>
      <th>Walc</th>
      <th>health</th>
      <th>absences</th>
      <th>G1</th>
      <th>G2</th>
      <th>G3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>GP</td>
      <td>F</td>
      <td>18</td>
      <td>U</td>
      <td>GT3</td>
      <td>A</td>
      <td>4</td>
      <td>4</td>
      <td>at_home</td>
      <td>teacher</td>
      <td>...</td>
      <td>4</td>
      <td>3</td>
      <td>4</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>4</td>
      <td>0</td>
      <td>11</td>
      <td>11</td>
    </tr>
    <tr>
      <th>1</th>
      <td>GP</td>
      <td>F</td>
      <td>17</td>
      <td>U</td>
      <td>GT3</td>
      <td>T</td>
      <td>1</td>
      <td>1</td>
      <td>at_home</td>
      <td>other</td>
      <td>...</td>
      <td>5</td>
      <td>3</td>
      <td>3</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>9</td>
      <td>11</td>
      <td>11</td>
    </tr>
    <tr>
      <th>2</th>
      <td>GP</td>
      <td>F</td>
      <td>15</td>
      <td>U</td>
      <td>LE3</td>
      <td>T</td>
      <td>1</td>
      <td>1</td>
      <td>at_home</td>
      <td>other</td>
      <td>...</td>
      <td>4</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>3</td>
      <td>3</td>
      <td>6</td>
      <td>12</td>
      <td>13</td>
      <td>12</td>
    </tr>
    <tr>
      <th>3</th>
      <td>GP</td>
      <td>F</td>
      <td>15</td>
      <td>U</td>
      <td>GT3</td>
      <td>T</td>
      <td>4</td>
      <td>2</td>
      <td>health</td>
      <td>services</td>
      <td>...</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>0</td>
      <td>14</td>
      <td>14</td>
      <td>14</td>
    </tr>
    <tr>
      <th>4</th>
      <td>GP</td>
      <td>F</td>
      <td>16</td>
      <td>U</td>
      <td>GT3</td>
      <td>T</td>
      <td>3</td>
      <td>3</td>
      <td>other</td>
      <td>other</td>
      <td>...</td>
      <td>4</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>5</td>
      <td>0</td>
      <td>11</td>
      <td>13</td>
      <td>13</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 33 columns</p>
</div>




```python
d
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>school</th>
      <th>sex</th>
      <th>age</th>
      <th>address</th>
      <th>famsize</th>
      <th>Pstatus</th>
      <th>Medu</th>
      <th>Fedu</th>
      <th>Mjob</th>
      <th>Fjob</th>
      <th>...</th>
      <th>famrel</th>
      <th>freetime</th>
      <th>goout</th>
      <th>Dalc</th>
      <th>Walc</th>
      <th>health</th>
      <th>absences</th>
      <th>G1</th>
      <th>G2</th>
      <th>G3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>GP</td>
      <td>F</td>
      <td>18</td>
      <td>U</td>
      <td>GT3</td>
      <td>A</td>
      <td>4</td>
      <td>4</td>
      <td>at_home</td>
      <td>teacher</td>
      <td>...</td>
      <td>4</td>
      <td>3</td>
      <td>4</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>4</td>
      <td>0</td>
      <td>11</td>
      <td>11</td>
    </tr>
    <tr>
      <th>1</th>
      <td>GP</td>
      <td>F</td>
      <td>17</td>
      <td>U</td>
      <td>GT3</td>
      <td>T</td>
      <td>1</td>
      <td>1</td>
      <td>at_home</td>
      <td>other</td>
      <td>...</td>
      <td>5</td>
      <td>3</td>
      <td>3</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>9</td>
      <td>11</td>
      <td>11</td>
    </tr>
    <tr>
      <th>2</th>
      <td>GP</td>
      <td>F</td>
      <td>15</td>
      <td>U</td>
      <td>LE3</td>
      <td>T</td>
      <td>1</td>
      <td>1</td>
      <td>at_home</td>
      <td>other</td>
      <td>...</td>
      <td>4</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>3</td>
      <td>3</td>
      <td>6</td>
      <td>12</td>
      <td>13</td>
      <td>12</td>
    </tr>
    <tr>
      <th>3</th>
      <td>GP</td>
      <td>F</td>
      <td>15</td>
      <td>U</td>
      <td>GT3</td>
      <td>T</td>
      <td>4</td>
      <td>2</td>
      <td>health</td>
      <td>services</td>
      <td>...</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>0</td>
      <td>14</td>
      <td>14</td>
      <td>14</td>
    </tr>
    <tr>
      <th>4</th>
      <td>GP</td>
      <td>F</td>
      <td>16</td>
      <td>U</td>
      <td>GT3</td>
      <td>T</td>
      <td>3</td>
      <td>3</td>
      <td>other</td>
      <td>other</td>
      <td>...</td>
      <td>4</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>5</td>
      <td>0</td>
      <td>11</td>
      <td>13</td>
      <td>13</td>
    </tr>
    <tr>
      <th>5</th>
      <td>GP</td>
      <td>M</td>
      <td>16</td>
      <td>U</td>
      <td>LE3</td>
      <td>T</td>
      <td>4</td>
      <td>3</td>
      <td>services</td>
      <td>other</td>
      <td>...</td>
      <td>5</td>
      <td>4</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>5</td>
      <td>6</td>
      <td>12</td>
      <td>12</td>
      <td>13</td>
    </tr>
    <tr>
      <th>6</th>
      <td>GP</td>
      <td>M</td>
      <td>16</td>
      <td>U</td>
      <td>LE3</td>
      <td>T</td>
      <td>2</td>
      <td>2</td>
      <td>other</td>
      <td>other</td>
      <td>...</td>
      <td>4</td>
      <td>4</td>
      <td>4</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>0</td>
      <td>13</td>
      <td>12</td>
      <td>13</td>
    </tr>
    <tr>
      <th>7</th>
      <td>GP</td>
      <td>F</td>
      <td>17</td>
      <td>U</td>
      <td>GT3</td>
      <td>A</td>
      <td>4</td>
      <td>4</td>
      <td>other</td>
      <td>teacher</td>
      <td>...</td>
      <td>4</td>
      <td>1</td>
      <td>4</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>10</td>
      <td>13</td>
      <td>13</td>
    </tr>
    <tr>
      <th>8</th>
      <td>GP</td>
      <td>M</td>
      <td>15</td>
      <td>U</td>
      <td>LE3</td>
      <td>A</td>
      <td>3</td>
      <td>2</td>
      <td>services</td>
      <td>other</td>
      <td>...</td>
      <td>4</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>15</td>
      <td>16</td>
      <td>17</td>
    </tr>
    <tr>
      <th>9</th>
      <td>GP</td>
      <td>M</td>
      <td>15</td>
      <td>U</td>
      <td>GT3</td>
      <td>T</td>
      <td>3</td>
      <td>4</td>
      <td>other</td>
      <td>other</td>
      <td>...</td>
      <td>5</td>
      <td>5</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>0</td>
      <td>12</td>
      <td>12</td>
      <td>13</td>
    </tr>
    <tr>
      <th>10</th>
      <td>GP</td>
      <td>F</td>
      <td>15</td>
      <td>U</td>
      <td>GT3</td>
      <td>T</td>
      <td>4</td>
      <td>4</td>
      <td>teacher</td>
      <td>health</td>
      <td>...</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>14</td>
      <td>14</td>
      <td>14</td>
    </tr>
    <tr>
      <th>11</th>
      <td>GP</td>
      <td>F</td>
      <td>15</td>
      <td>U</td>
      <td>GT3</td>
      <td>T</td>
      <td>2</td>
      <td>1</td>
      <td>services</td>
      <td>other</td>
      <td>...</td>
      <td>5</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>10</td>
      <td>12</td>
      <td>13</td>
    </tr>
    <tr>
      <th>12</th>
      <td>GP</td>
      <td>M</td>
      <td>15</td>
      <td>U</td>
      <td>LE3</td>
      <td>T</td>
      <td>4</td>
      <td>4</td>
      <td>health</td>
      <td>services</td>
      <td>...</td>
      <td>4</td>
      <td>3</td>
      <td>3</td>
      <td>1</td>
      <td>3</td>
      <td>5</td>
      <td>0</td>
      <td>12</td>
      <td>13</td>
      <td>12</td>
    </tr>
    <tr>
      <th>13</th>
      <td>GP</td>
      <td>M</td>
      <td>15</td>
      <td>U</td>
      <td>GT3</td>
      <td>T</td>
      <td>4</td>
      <td>3</td>
      <td>teacher</td>
      <td>other</td>
      <td>...</td>
      <td>5</td>
      <td>4</td>
      <td>3</td>
      <td>1</td>
      <td>2</td>
      <td>3</td>
      <td>0</td>
      <td>12</td>
      <td>12</td>
      <td>13</td>
    </tr>
    <tr>
      <th>14</th>
      <td>GP</td>
      <td>M</td>
      <td>15</td>
      <td>U</td>
      <td>GT3</td>
      <td>A</td>
      <td>2</td>
      <td>2</td>
      <td>other</td>
      <td>other</td>
      <td>...</td>
      <td>4</td>
      <td>5</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>0</td>
      <td>14</td>
      <td>14</td>
      <td>15</td>
    </tr>
    <tr>
      <th>15</th>
      <td>GP</td>
      <td>F</td>
      <td>16</td>
      <td>U</td>
      <td>GT3</td>
      <td>T</td>
      <td>4</td>
      <td>4</td>
      <td>health</td>
      <td>other</td>
      <td>...</td>
      <td>4</td>
      <td>4</td>
      <td>4</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>6</td>
      <td>17</td>
      <td>17</td>
      <td>17</td>
    </tr>
    <tr>
      <th>16</th>
      <td>GP</td>
      <td>F</td>
      <td>16</td>
      <td>U</td>
      <td>GT3</td>
      <td>T</td>
      <td>4</td>
      <td>4</td>
      <td>services</td>
      <td>services</td>
      <td>...</td>
      <td>3</td>
      <td>2</td>
      <td>3</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>10</td>
      <td>13</td>
      <td>13</td>
      <td>14</td>
    </tr>
    <tr>
      <th>17</th>
      <td>GP</td>
      <td>F</td>
      <td>16</td>
      <td>U</td>
      <td>GT3</td>
      <td>T</td>
      <td>3</td>
      <td>3</td>
      <td>other</td>
      <td>other</td>
      <td>...</td>
      <td>5</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>4</td>
      <td>2</td>
      <td>13</td>
      <td>14</td>
      <td>14</td>
    </tr>
    <tr>
      <th>18</th>
      <td>GP</td>
      <td>M</td>
      <td>17</td>
      <td>U</td>
      <td>GT3</td>
      <td>T</td>
      <td>3</td>
      <td>2</td>
      <td>services</td>
      <td>services</td>
      <td>...</td>
      <td>5</td>
      <td>5</td>
      <td>5</td>
      <td>2</td>
      <td>4</td>
      <td>5</td>
      <td>2</td>
      <td>8</td>
      <td>8</td>
      <td>7</td>
    </tr>
    <tr>
      <th>19</th>
      <td>GP</td>
      <td>M</td>
      <td>16</td>
      <td>U</td>
      <td>LE3</td>
      <td>T</td>
      <td>4</td>
      <td>3</td>
      <td>health</td>
      <td>other</td>
      <td>...</td>
      <td>3</td>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>3</td>
      <td>5</td>
      <td>6</td>
      <td>12</td>
      <td>12</td>
      <td>12</td>
    </tr>
    <tr>
      <th>20</th>
      <td>GP</td>
      <td>M</td>
      <td>15</td>
      <td>U</td>
      <td>GT3</td>
      <td>T</td>
      <td>4</td>
      <td>3</td>
      <td>teacher</td>
      <td>other</td>
      <td>...</td>
      <td>4</td>
      <td>4</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>12</td>
      <td>13</td>
      <td>14</td>
    </tr>
    <tr>
      <th>21</th>
      <td>GP</td>
      <td>M</td>
      <td>15</td>
      <td>U</td>
      <td>GT3</td>
      <td>T</td>
      <td>4</td>
      <td>4</td>
      <td>health</td>
      <td>health</td>
      <td>...</td>
      <td>5</td>
      <td>4</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>0</td>
      <td>11</td>
      <td>12</td>
      <td>12</td>
    </tr>
    <tr>
      <th>22</th>
      <td>GP</td>
      <td>M</td>
      <td>16</td>
      <td>U</td>
      <td>LE3</td>
      <td>T</td>
      <td>4</td>
      <td>2</td>
      <td>teacher</td>
      <td>other</td>
      <td>...</td>
      <td>4</td>
      <td>5</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>5</td>
      <td>0</td>
      <td>12</td>
      <td>13</td>
      <td>14</td>
    </tr>
    <tr>
      <th>23</th>
      <td>GP</td>
      <td>M</td>
      <td>16</td>
      <td>U</td>
      <td>LE3</td>
      <td>T</td>
      <td>2</td>
      <td>2</td>
      <td>other</td>
      <td>other</td>
      <td>...</td>
      <td>5</td>
      <td>4</td>
      <td>4</td>
      <td>2</td>
      <td>4</td>
      <td>5</td>
      <td>2</td>
      <td>10</td>
      <td>10</td>
      <td>10</td>
    </tr>
    <tr>
      <th>24</th>
      <td>GP</td>
      <td>F</td>
      <td>15</td>
      <td>R</td>
      <td>GT3</td>
      <td>T</td>
      <td>2</td>
      <td>4</td>
      <td>services</td>
      <td>health</td>
      <td>...</td>
      <td>4</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>2</td>
      <td>10</td>
      <td>11</td>
      <td>10</td>
    </tr>
    <tr>
      <th>25</th>
      <td>GP</td>
      <td>F</td>
      <td>16</td>
      <td>U</td>
      <td>GT3</td>
      <td>T</td>
      <td>2</td>
      <td>2</td>
      <td>services</td>
      <td>services</td>
      <td>...</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>5</td>
      <td>6</td>
      <td>10</td>
      <td>11</td>
      <td>12</td>
    </tr>
    <tr>
      <th>26</th>
      <td>GP</td>
      <td>M</td>
      <td>15</td>
      <td>U</td>
      <td>GT3</td>
      <td>T</td>
      <td>2</td>
      <td>2</td>
      <td>other</td>
      <td>other</td>
      <td>...</td>
      <td>4</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>5</td>
      <td>8</td>
      <td>11</td>
      <td>12</td>
      <td>12</td>
    </tr>
    <tr>
      <th>27</th>
      <td>GP</td>
      <td>M</td>
      <td>15</td>
      <td>U</td>
      <td>GT3</td>
      <td>T</td>
      <td>4</td>
      <td>2</td>
      <td>health</td>
      <td>services</td>
      <td>...</td>
      <td>2</td>
      <td>2</td>
      <td>4</td>
      <td>2</td>
      <td>4</td>
      <td>1</td>
      <td>0</td>
      <td>11</td>
      <td>11</td>
      <td>11</td>
    </tr>
    <tr>
      <th>28</th>
      <td>GP</td>
      <td>M</td>
      <td>16</td>
      <td>U</td>
      <td>LE3</td>
      <td>A</td>
      <td>3</td>
      <td>4</td>
      <td>services</td>
      <td>other</td>
      <td>...</td>
      <td>5</td>
      <td>3</td>
      <td>3</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>2</td>
      <td>12</td>
      <td>12</td>
      <td>13</td>
    </tr>
    <tr>
      <th>29</th>
      <td>GP</td>
      <td>M</td>
      <td>16</td>
      <td>U</td>
      <td>GT3</td>
      <td>T</td>
      <td>4</td>
      <td>4</td>
      <td>teacher</td>
      <td>teacher</td>
      <td>...</td>
      <td>4</td>
      <td>4</td>
      <td>5</td>
      <td>5</td>
      <td>5</td>
      <td>5</td>
      <td>4</td>
      <td>12</td>
      <td>11</td>
      <td>12</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>619</th>
      <td>MS</td>
      <td>F</td>
      <td>18</td>
      <td>U</td>
      <td>GT3</td>
      <td>T</td>
      <td>3</td>
      <td>3</td>
      <td>services</td>
      <td>services</td>
      <td>...</td>
      <td>4</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>3</td>
      <td>6</td>
      <td>13</td>
      <td>12</td>
      <td>13</td>
    </tr>
    <tr>
      <th>620</th>
      <td>MS</td>
      <td>F</td>
      <td>17</td>
      <td>U</td>
      <td>LE3</td>
      <td>T</td>
      <td>4</td>
      <td>4</td>
      <td>at_home</td>
      <td>at_home</td>
      <td>...</td>
      <td>2</td>
      <td>3</td>
      <td>4</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>4</td>
      <td>15</td>
      <td>14</td>
      <td>15</td>
    </tr>
    <tr>
      <th>621</th>
      <td>MS</td>
      <td>F</td>
      <td>17</td>
      <td>R</td>
      <td>GT3</td>
      <td>T</td>
      <td>1</td>
      <td>2</td>
      <td>other</td>
      <td>services</td>
      <td>...</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>3</td>
      <td>0</td>
      <td>13</td>
      <td>13</td>
      <td>13</td>
    </tr>
    <tr>
      <th>622</th>
      <td>MS</td>
      <td>M</td>
      <td>18</td>
      <td>R</td>
      <td>GT3</td>
      <td>T</td>
      <td>1</td>
      <td>3</td>
      <td>at_home</td>
      <td>other</td>
      <td>...</td>
      <td>3</td>
      <td>3</td>
      <td>4</td>
      <td>2</td>
      <td>4</td>
      <td>3</td>
      <td>0</td>
      <td>8</td>
      <td>10</td>
      <td>9</td>
    </tr>
    <tr>
      <th>623</th>
      <td>MS</td>
      <td>M</td>
      <td>18</td>
      <td>U</td>
      <td>LE3</td>
      <td>T</td>
      <td>4</td>
      <td>4</td>
      <td>teacher</td>
      <td>services</td>
      <td>...</td>
      <td>4</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>5</td>
      <td>0</td>
      <td>15</td>
      <td>16</td>
      <td>16</td>
    </tr>
    <tr>
      <th>624</th>
      <td>MS</td>
      <td>F</td>
      <td>17</td>
      <td>R</td>
      <td>GT3</td>
      <td>T</td>
      <td>1</td>
      <td>1</td>
      <td>other</td>
      <td>services</td>
      <td>...</td>
      <td>5</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>0</td>
      <td>8</td>
      <td>8</td>
      <td>9</td>
    </tr>
    <tr>
      <th>625</th>
      <td>MS</td>
      <td>F</td>
      <td>18</td>
      <td>U</td>
      <td>GT3</td>
      <td>T</td>
      <td>2</td>
      <td>3</td>
      <td>at_home</td>
      <td>services</td>
      <td>...</td>
      <td>5</td>
      <td>2</td>
      <td>3</td>
      <td>1</td>
      <td>2</td>
      <td>4</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>10</td>
    </tr>
    <tr>
      <th>626</th>
      <td>MS</td>
      <td>F</td>
      <td>18</td>
      <td>R</td>
      <td>GT3</td>
      <td>T</td>
      <td>4</td>
      <td>4</td>
      <td>other</td>
      <td>teacher</td>
      <td>...</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>4</td>
      <td>2</td>
      <td>5</td>
      <td>0</td>
      <td>7</td>
      <td>5</td>
      <td>0</td>
    </tr>
    <tr>
      <th>627</th>
      <td>MS</td>
      <td>M</td>
      <td>18</td>
      <td>R</td>
      <td>LE3</td>
      <td>T</td>
      <td>1</td>
      <td>2</td>
      <td>at_home</td>
      <td>services</td>
      <td>...</td>
      <td>4</td>
      <td>3</td>
      <td>3</td>
      <td>2</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>9</td>
      <td>10</td>
      <td>10</td>
    </tr>
    <tr>
      <th>628</th>
      <td>MS</td>
      <td>F</td>
      <td>17</td>
      <td>U</td>
      <td>GT3</td>
      <td>T</td>
      <td>2</td>
      <td>2</td>
      <td>other</td>
      <td>at_home</td>
      <td>...</td>
      <td>3</td>
      <td>4</td>
      <td>3</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>8</td>
      <td>10</td>
      <td>11</td>
      <td>12</td>
    </tr>
    <tr>
      <th>629</th>
      <td>MS</td>
      <td>F</td>
      <td>17</td>
      <td>R</td>
      <td>GT3</td>
      <td>T</td>
      <td>1</td>
      <td>2</td>
      <td>other</td>
      <td>other</td>
      <td>...</td>
      <td>3</td>
      <td>5</td>
      <td>5</td>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>4</td>
      <td>7</td>
      <td>8</td>
      <td>9</td>
    </tr>
    <tr>
      <th>630</th>
      <td>MS</td>
      <td>F</td>
      <td>18</td>
      <td>R</td>
      <td>LE3</td>
      <td>T</td>
      <td>4</td>
      <td>4</td>
      <td>other</td>
      <td>other</td>
      <td>...</td>
      <td>5</td>
      <td>4</td>
      <td>4</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>15</td>
      <td>17</td>
      <td>17</td>
    </tr>
    <tr>
      <th>631</th>
      <td>MS</td>
      <td>F</td>
      <td>18</td>
      <td>R</td>
      <td>GT3</td>
      <td>T</td>
      <td>1</td>
      <td>1</td>
      <td>other</td>
      <td>other</td>
      <td>...</td>
      <td>4</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>4</td>
      <td>4</td>
      <td>10</td>
      <td>11</td>
      <td>12</td>
    </tr>
    <tr>
      <th>632</th>
      <td>MS</td>
      <td>F</td>
      <td>19</td>
      <td>R</td>
      <td>GT3</td>
      <td>T</td>
      <td>1</td>
      <td>1</td>
      <td>at_home</td>
      <td>other</td>
      <td>...</td>
      <td>4</td>
      <td>3</td>
      <td>3</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>4</td>
      <td>7</td>
      <td>8</td>
      <td>9</td>
    </tr>
    <tr>
      <th>633</th>
      <td>MS</td>
      <td>F</td>
      <td>18</td>
      <td>R</td>
      <td>LE3</td>
      <td>T</td>
      <td>4</td>
      <td>4</td>
      <td>teacher</td>
      <td>services</td>
      <td>...</td>
      <td>5</td>
      <td>4</td>
      <td>3</td>
      <td>3</td>
      <td>4</td>
      <td>2</td>
      <td>1</td>
      <td>13</td>
      <td>14</td>
      <td>14</td>
    </tr>
    <tr>
      <th>634</th>
      <td>MS</td>
      <td>F</td>
      <td>18</td>
      <td>U</td>
      <td>GT3</td>
      <td>T</td>
      <td>3</td>
      <td>3</td>
      <td>other</td>
      <td>other</td>
      <td>...</td>
      <td>4</td>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>16</td>
      <td>16</td>
      <td>16</td>
    </tr>
    <tr>
      <th>635</th>
      <td>MS</td>
      <td>F</td>
      <td>17</td>
      <td>R</td>
      <td>GT3</td>
      <td>T</td>
      <td>3</td>
      <td>1</td>
      <td>at_home</td>
      <td>other</td>
      <td>...</td>
      <td>4</td>
      <td>5</td>
      <td>4</td>
      <td>2</td>
      <td>3</td>
      <td>1</td>
      <td>10</td>
      <td>8</td>
      <td>9</td>
      <td>9</td>
    </tr>
    <tr>
      <th>636</th>
      <td>MS</td>
      <td>M</td>
      <td>18</td>
      <td>U</td>
      <td>GT3</td>
      <td>T</td>
      <td>4</td>
      <td>4</td>
      <td>teacher</td>
      <td>teacher</td>
      <td>...</td>
      <td>3</td>
      <td>2</td>
      <td>4</td>
      <td>1</td>
      <td>4</td>
      <td>2</td>
      <td>4</td>
      <td>17</td>
      <td>18</td>
      <td>19</td>
    </tr>
    <tr>
      <th>637</th>
      <td>MS</td>
      <td>M</td>
      <td>18</td>
      <td>R</td>
      <td>GT3</td>
      <td>T</td>
      <td>2</td>
      <td>1</td>
      <td>other</td>
      <td>other</td>
      <td>...</td>
      <td>4</td>
      <td>4</td>
      <td>3</td>
      <td>1</td>
      <td>3</td>
      <td>5</td>
      <td>0</td>
      <td>7</td>
      <td>7</td>
      <td>0</td>
    </tr>
    <tr>
      <th>638</th>
      <td>MS</td>
      <td>M</td>
      <td>17</td>
      <td>U</td>
      <td>GT3</td>
      <td>T</td>
      <td>2</td>
      <td>3</td>
      <td>other</td>
      <td>services</td>
      <td>...</td>
      <td>4</td>
      <td>4</td>
      <td>3</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>4</td>
      <td>14</td>
      <td>15</td>
      <td>16</td>
    </tr>
    <tr>
      <th>639</th>
      <td>MS</td>
      <td>M</td>
      <td>19</td>
      <td>R</td>
      <td>GT3</td>
      <td>T</td>
      <td>1</td>
      <td>1</td>
      <td>other</td>
      <td>services</td>
      <td>...</td>
      <td>4</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>5</td>
      <td>0</td>
      <td>5</td>
      <td>8</td>
      <td>0</td>
    </tr>
    <tr>
      <th>640</th>
      <td>MS</td>
      <td>M</td>
      <td>18</td>
      <td>R</td>
      <td>GT3</td>
      <td>T</td>
      <td>4</td>
      <td>2</td>
      <td>other</td>
      <td>other</td>
      <td>...</td>
      <td>5</td>
      <td>4</td>
      <td>3</td>
      <td>4</td>
      <td>3</td>
      <td>3</td>
      <td>0</td>
      <td>7</td>
      <td>7</td>
      <td>0</td>
    </tr>
    <tr>
      <th>641</th>
      <td>MS</td>
      <td>F</td>
      <td>18</td>
      <td>R</td>
      <td>GT3</td>
      <td>T</td>
      <td>2</td>
      <td>2</td>
      <td>at_home</td>
      <td>other</td>
      <td>...</td>
      <td>5</td>
      <td>3</td>
      <td>3</td>
      <td>1</td>
      <td>3</td>
      <td>4</td>
      <td>0</td>
      <td>14</td>
      <td>17</td>
      <td>15</td>
    </tr>
    <tr>
      <th>642</th>
      <td>MS</td>
      <td>F</td>
      <td>17</td>
      <td>U</td>
      <td>GT3</td>
      <td>T</td>
      <td>4</td>
      <td>3</td>
      <td>teacher</td>
      <td>other</td>
      <td>...</td>
      <td>5</td>
      <td>5</td>
      <td>4</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>6</td>
      <td>9</td>
      <td>11</td>
    </tr>
    <tr>
      <th>643</th>
      <td>MS</td>
      <td>F</td>
      <td>18</td>
      <td>R</td>
      <td>GT3</td>
      <td>T</td>
      <td>4</td>
      <td>4</td>
      <td>teacher</td>
      <td>at_home</td>
      <td>...</td>
      <td>4</td>
      <td>4</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>5</td>
      <td>4</td>
      <td>7</td>
      <td>9</td>
      <td>10</td>
    </tr>
    <tr>
      <th>644</th>
      <td>MS</td>
      <td>F</td>
      <td>19</td>
      <td>R</td>
      <td>GT3</td>
      <td>T</td>
      <td>2</td>
      <td>3</td>
      <td>services</td>
      <td>other</td>
      <td>...</td>
      <td>5</td>
      <td>4</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>5</td>
      <td>4</td>
      <td>10</td>
      <td>11</td>
      <td>10</td>
    </tr>
    <tr>
      <th>645</th>
      <td>MS</td>
      <td>F</td>
      <td>18</td>
      <td>U</td>
      <td>LE3</td>
      <td>T</td>
      <td>3</td>
      <td>1</td>
      <td>teacher</td>
      <td>services</td>
      <td>...</td>
      <td>4</td>
      <td>3</td>
      <td>4</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>16</td>
    </tr>
    <tr>
      <th>646</th>
      <td>MS</td>
      <td>F</td>
      <td>18</td>
      <td>U</td>
      <td>GT3</td>
      <td>T</td>
      <td>1</td>
      <td>1</td>
      <td>other</td>
      <td>other</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>6</td>
      <td>11</td>
      <td>12</td>
      <td>9</td>
    </tr>
    <tr>
      <th>647</th>
      <td>MS</td>
      <td>M</td>
      <td>17</td>
      <td>U</td>
      <td>LE3</td>
      <td>T</td>
      <td>3</td>
      <td>1</td>
      <td>services</td>
      <td>services</td>
      <td>...</td>
      <td>2</td>
      <td>4</td>
      <td>5</td>
      <td>3</td>
      <td>4</td>
      <td>2</td>
      <td>6</td>
      <td>10</td>
      <td>10</td>
      <td>10</td>
    </tr>
    <tr>
      <th>648</th>
      <td>MS</td>
      <td>M</td>
      <td>18</td>
      <td>R</td>
      <td>LE3</td>
      <td>T</td>
      <td>3</td>
      <td>2</td>
      <td>services</td>
      <td>other</td>
      <td>...</td>
      <td>4</td>
      <td>4</td>
      <td>1</td>
      <td>3</td>
      <td>4</td>
      <td>5</td>
      <td>4</td>
      <td>10</td>
      <td>11</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
<p>649 rows × 33 columns</p>
</div>




```python
d['pass'] = d.apply(lambda row: 1 if (row['G1']+row['G2']+row['G3']) >= 35 else 0, axis=1)
```


```python
d.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>school</th>
      <th>sex</th>
      <th>age</th>
      <th>address</th>
      <th>famsize</th>
      <th>Pstatus</th>
      <th>Medu</th>
      <th>Fedu</th>
      <th>Mjob</th>
      <th>Fjob</th>
      <th>...</th>
      <th>freetime</th>
      <th>goout</th>
      <th>Dalc</th>
      <th>Walc</th>
      <th>health</th>
      <th>absences</th>
      <th>G1</th>
      <th>G2</th>
      <th>G3</th>
      <th>pass</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>GP</td>
      <td>F</td>
      <td>18</td>
      <td>U</td>
      <td>GT3</td>
      <td>A</td>
      <td>4</td>
      <td>4</td>
      <td>at_home</td>
      <td>teacher</td>
      <td>...</td>
      <td>3</td>
      <td>4</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>4</td>
      <td>0</td>
      <td>11</td>
      <td>11</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>GP</td>
      <td>F</td>
      <td>17</td>
      <td>U</td>
      <td>GT3</td>
      <td>T</td>
      <td>1</td>
      <td>1</td>
      <td>at_home</td>
      <td>other</td>
      <td>...</td>
      <td>3</td>
      <td>3</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>9</td>
      <td>11</td>
      <td>11</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>GP</td>
      <td>F</td>
      <td>15</td>
      <td>U</td>
      <td>LE3</td>
      <td>T</td>
      <td>1</td>
      <td>1</td>
      <td>at_home</td>
      <td>other</td>
      <td>...</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>3</td>
      <td>3</td>
      <td>6</td>
      <td>12</td>
      <td>13</td>
      <td>12</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>GP</td>
      <td>F</td>
      <td>15</td>
      <td>U</td>
      <td>GT3</td>
      <td>T</td>
      <td>4</td>
      <td>2</td>
      <td>health</td>
      <td>services</td>
      <td>...</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>0</td>
      <td>14</td>
      <td>14</td>
      <td>14</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>GP</td>
      <td>F</td>
      <td>16</td>
      <td>U</td>
      <td>GT3</td>
      <td>T</td>
      <td>3</td>
      <td>3</td>
      <td>other</td>
      <td>other</td>
      <td>...</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>5</td>
      <td>0</td>
      <td>11</td>
      <td>13</td>
      <td>13</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 34 columns</p>
</div>




```python
d = d.drop(['G1', 'G2', 'G3'], axis=1)
```


```python
d.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>school</th>
      <th>sex</th>
      <th>age</th>
      <th>address</th>
      <th>famsize</th>
      <th>Pstatus</th>
      <th>Medu</th>
      <th>Fedu</th>
      <th>Mjob</th>
      <th>Fjob</th>
      <th>...</th>
      <th>internet</th>
      <th>romantic</th>
      <th>famrel</th>
      <th>freetime</th>
      <th>goout</th>
      <th>Dalc</th>
      <th>Walc</th>
      <th>health</th>
      <th>absences</th>
      <th>pass</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>GP</td>
      <td>F</td>
      <td>18</td>
      <td>U</td>
      <td>GT3</td>
      <td>A</td>
      <td>4</td>
      <td>4</td>
      <td>at_home</td>
      <td>teacher</td>
      <td>...</td>
      <td>no</td>
      <td>no</td>
      <td>4</td>
      <td>3</td>
      <td>4</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>4</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>GP</td>
      <td>F</td>
      <td>17</td>
      <td>U</td>
      <td>GT3</td>
      <td>T</td>
      <td>1</td>
      <td>1</td>
      <td>at_home</td>
      <td>other</td>
      <td>...</td>
      <td>yes</td>
      <td>no</td>
      <td>5</td>
      <td>3</td>
      <td>3</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>GP</td>
      <td>F</td>
      <td>15</td>
      <td>U</td>
      <td>LE3</td>
      <td>T</td>
      <td>1</td>
      <td>1</td>
      <td>at_home</td>
      <td>other</td>
      <td>...</td>
      <td>yes</td>
      <td>no</td>
      <td>4</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>3</td>
      <td>3</td>
      <td>6</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>GP</td>
      <td>F</td>
      <td>15</td>
      <td>U</td>
      <td>GT3</td>
      <td>T</td>
      <td>4</td>
      <td>2</td>
      <td>health</td>
      <td>services</td>
      <td>...</td>
      <td>yes</td>
      <td>yes</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>GP</td>
      <td>F</td>
      <td>16</td>
      <td>U</td>
      <td>GT3</td>
      <td>T</td>
      <td>3</td>
      <td>3</td>
      <td>other</td>
      <td>other</td>
      <td>...</td>
      <td>no</td>
      <td>no</td>
      <td>4</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>5</td>
      <td>0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 31 columns</p>
</div>




```python
d = pd.get_dummies(d, ["sex", "school", "address", "famsize",
                       "Pstatus", "Mjob", "Fjob", "reason",
                       "guardian", "schoolsup", "famsup",
                       "paid", "activities", "nursery",
                       "higher", "internet", "romantic"])
```


```python
d.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>age</th>
      <th>Medu</th>
      <th>Fedu</th>
      <th>traveltime</th>
      <th>studytime</th>
      <th>failures</th>
      <th>famrel</th>
      <th>freetime</th>
      <th>goout</th>
      <th>Dalc</th>
      <th>...</th>
      <th>activities_no</th>
      <th>activities_yes</th>
      <th>nursery_no</th>
      <th>nursery_yes</th>
      <th>higher_no</th>
      <th>higher_yes</th>
      <th>internet_no</th>
      <th>internet_yes</th>
      <th>romantic_no</th>
      <th>romantic_yes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>18</td>
      <td>4</td>
      <td>4</td>
      <td>2</td>
      <td>2</td>
      <td>0</td>
      <td>4</td>
      <td>3</td>
      <td>4</td>
      <td>1</td>
      <td>...</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>17</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>5</td>
      <td>3</td>
      <td>3</td>
      <td>1</td>
      <td>...</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>15</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>4</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>...</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>15</td>
      <td>4</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>0</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>16</td>
      <td>3</td>
      <td>3</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>4</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>...</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 57 columns</p>
</div>




```python
from sklearn import tree
t = tree.DecisionTreeClassifier(criterion="entropy", max_depth=20)
```


```python
d_att = d.drop(['pass'], axis=1)
d_pass = d['pass']
```


```python
t = t.fit(d_att, d_pass)
```


```python
import graphviz
graph_data = tree.export_graphviz(t, out_file=None, label="all",
                                  impurity=False, proportion=True,
                                  feature_names=list(d_att),
                                  class_names=["fail", "pass"],
                                  filled=True, rounded=True)
graph = graphviz.Source(graph_data)
graph
```




![svg](/images/student-decision-tree.svg)




```python
t.score(d_att, d_pass)
```




    1.0




```python
d = d.sample(frac=1.0)
```


```python
d_att = d.drop(['pass'], axis=1)
d_pass = d['pass']

```


```python
d_train_att, d_train_pass = d_att[:500], d_pass[:500]
d_test_att, d_test_pass = d_att[500:], d_pass[500:]

```


```python
len(d_train_att)
```




    500




```python
len(d_test_att)

```




    149




```python
t = t.fit(d_train_att, d_train_pass)
```


```python
t.score(d_test_att, d_test_pass)
```




    0.6912751677852349




```python
from sklearn.model_selection import cross_val_score
```


```python
scores = cross_val_score(t, d_att, d_pass, cv=5)
```


```python
scores.mean()
```




    0.6456911879173551




```python
for max_depth in range(1, 20):
    t = tree.DecisionTreeClassifier(criterion="entropy", max_depth=max_depth)
    scores = cross_val_score(t, d_att, d_pass, cv=5)
    print("Max depth: {}, score = {}".format(max_depth, scores.mean()))
```

    Max depth: 1, score = 0.6378328257930601
    Max depth: 2, score = 0.68713660799228
    Max depth: 3, score = 0.7041198614392294
    Max depth: 4, score = 0.6809471657403486
    Max depth: 5, score = 0.6948648759371286
    Max depth: 6, score = 0.6762719687555477
    Max depth: 7, score = 0.6810539540346038
    Max depth: 8, score = 0.6732311880083937
    Max depth: 9, score = 0.7025458038026829
    Max depth: 10, score = 0.6763075648536326
    Max depth: 11, score = 0.6624733370659166
    Max depth: 12, score = 0.6470637771010573
    Max depth: 13, score = 0.650164916449312
    Max depth: 14, score = 0.6517745701839435
    Max depth: 15, score = 0.645596871913222
    Max depth: 16, score = 0.6549586457095777
    Max depth: 17, score = 0.6642015230760127
    Max depth: 18, score = 0.6518345646305881
    Max depth: 19, score = 0.6687101193971423



```python

```

## YouTube Spam - Decision Trees


```python
import pandas as pd
```


```python
d = pd.read_csv("/aicamp/Youtube01-Psy.csv")
```


```python
d.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>COMMENT_ID</th>
      <th>AUTHOR</th>
      <th>DATE</th>
      <th>CONTENT</th>
      <th>CLASS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>LZQPQhLyRh80UYxNuaDWhIGQYNQ96IuCg-AYWqNPjpU</td>
      <td>Julius NM</td>
      <td>2013-11-07T06:20:48</td>
      <td>Huh, anyway check out this you[tube] channel</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>LZQPQhLyRh_C2cTtd9MvFRJedxydaVW-2sNg5Diuo4A</td>
      <td>adam riyati</td>
      <td>2013-11-07T12:37:15</td>
      <td>Hey guys check out my new channel and our firs...</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>LZQPQhLyRh9MSZYnf8djyk0gEF9BHDPYrrK-qCczIY8</td>
      <td>Evgeny Murashkin</td>
      <td>2013-11-08T17:34:21</td>
      <td>just for test I have to say httphidden</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>z13fwbwp1oujthgqj04chlngpvzmtt3r3dw</td>
      <td>GsMega</td>
      <td>2013-11-10T16:05:38</td>
      <td>watch?v=hidden  Check this out .Ôªø</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>LZQPQhLyRh9-wNRtlZDM90f1k0BrdVdJyN_YsaSwfxc</td>
      <td>Jason Haddad</td>
      <td>2013-11-26T02:55:11</td>
      <td>Hey, check out my new website!! This site is a...</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
len(d.query('CLASS == 1'))
```




    169




```python
len(d.query('CLASS == 0'))
```




    152




```python
from sklearn.feature_extraction.text import CountVectorizer
```


```python
vectorizer = CountVectorizer(ngram_range=(1,2),
                             stop_words='english')
```


```python
dvec = vectorizer.fit_transform(d['CONTENT'])
```


```python
dvec
```




    <321x2494 sparse matrix of type '<class 'numpy.int64'>'
    	with 3873 stored elements in Compressed Sparse Row format>




```python
dshuf = d.sample(frac=1.0)
```


```python
d_att = vectorizer.fit_transform(dshuf['CONTENT'])
d_label = dshuf['CLASS']
```


```python
from sklearn import tree
```


```python
t = tree.DecisionTreeClassifier(criterion='entropy', max_depth=5)
```


```python
t = t.fit(d_att, d_label)
```


```python
# visualize tree
import graphviz
dot_data = tree.export_graphviz(t, out_file=None, label="all",
                                impurity=False, proportion=True,
                                class_names=["notspam","spam"],
                                feature_names=sorted(vectorizer.vocabulary_.keys()),
                                filled=True, rounded=True)
graph = graphviz.Source(dot_data)
graph
```




![svg](/images/youtube-decision-tree.svg)






```python

```

