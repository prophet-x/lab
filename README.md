# lab

package wordcount;

import java.io.IOException;
import java.util.StringTokenizer;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;

public class WordCount {

  public static class TokenizerMapper
       extends Mapper<Object, Text, Text, IntWritable>{

    private final static IntWritable one = new IntWritable(1);
    private Text word = new Text();

    public void map(Object key, Text value, Context context) throws IOException, InterruptedException {
      StringTokenizer itr = new StringTokenizer(value.toString());
      while (itr.hasMoreTokens()) {
        word.set(itr.nextToken());
        context.write(word, one);
      }
    }
  }

  public static class IntSumReducer
       extends Reducer<Text,IntWritable,Text,IntWritable> {
    private IntWritable result = new IntWritable();

    public void reduce(Text key, Iterable<IntWritable> values,
                       Context context
                       ) throws IOException, InterruptedException {
      int sum = 0;
      for (IntWritable val : values) {
        sum += val.get();
      }
      result.set(sum);
      context.write(key, result);
    }
  }

  public static void main(String[] args) throws Exception {
    Configuration conf = new Configuration();
    Job job = Job.getInstance(conf, "word count");
    job.setJarByClass(WordCount.class);
    job.setMapperClass(TokenizerMapper.class);
    job.setCombinerClass(IntSumReducer.class);
    job.setReducerClass(IntSumReducer.class);
    job.setOutputKeyClass(Text.class);
    job.setOutputValueClass(IntWritable.class);
    FileInputFormat.addInputPath(job, new Path(args[0]));
    FileOutputFormat.setOutputPath(job, new Path(args[1]));
    System.exit(job.waitForCompletion(true) ? 0 : 1);
  }
}


create 'flight','finfo','fsch'
put 'flight',1,'finfo:source','pune'
scan 'flight'
alter 'flight',NAME=>'revenue'
put 'flight',4,'revenue:rs','45000'
alter 'flight',NAME=>'revenue',METHOD=>'delete'
# drop
create 'tb1','cf'
disable 'tb1'
drop 'tb1'
get 'flight','1',COLUMN=>'finfo:source'
# external table
create table empdbnew(eno int, ename string, esal int) row format delimited fields terminated
by ',' stored as textfile;
load data local inpath '/home/hduser/Desktop/empdbnew.txt' into table empdbnew;
CREATE external TABLE hbase_flight_new(fno int, fsource string,fdest string,fsh_at string,fsh_dt string,fsch_delay
string,delay int)
STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
WITH SERDEPROPERTIES ("hbase.columns.mapping" =
":key,finfo:source,finfo:dest,fsch:at,fsch:dt,fsch:delay,delay:dl")
TBLPROPERTIES ("hbase.table.name" = "flight");
CREATE INDEX hbasefltnew_index
ON TABLE hbase_flight_new (delay)
AS 'org.apache.hadoop.hive.ql.index.compact.CompactIndexHandler'
WITH DEFERRED REBUILD;
SHOW INDEX ON hbase_flight_new;
# join
> SELECT eno, ename, empno, empgrade FROM empdbnew JOIN empinfo ON eno = empno;

# 4th assignment
data.iloc[12:20,[2,5,11,13]]
merged = data.append(newdata)
Y = data.drop(['Type','comment','Category'], axis = 1)
Y = Z.sort_values(by='comment', ascending = False)
Y = Z.T
Z.melt(id_vars='Category')

# 5th assignment
air.isnull().sum()
A = air.dropna()
A = air.fillna(0)
A = air.fillna(method='ffill')
A = air['Ozone'].replace(np.NaN,air['Ozone'].mean())
from sklearn.impute import SimpleImputer
imp = SimpleImputer(missing_values=np.nan,strategy='mean')
A = imp.fit_transform(air)
from sklearn.model_selection import train_test_split
train, test = train_test_split(A)
import matplotlib.pyplot as plt
X = df['Temp']
Y = df['Ozone']
plt.scatter(X,Y)
import matplotlib.pyplot as plt
import seaborn as sb
bxplt = sb.boxplot(df["target"],df["chol"])
plt.show()

 --> air quality
df["type"].unique()
types = {
    "Residential": "R",
    "Residential and others": "RO",
    "Residential, Rural and other Areas": "RRO",
    "Industrial Area": "I",
    "Industrial Areas": "I",
    "Industrial": "I",
    "Sensitive Area": "S",
    "Sensitive Areas": "S",
    "Sensitive": "S",
    "NaN": "RRO"
}
df.type = df.type.replace(types)
import numpy as np
from sklearn.impute import SimpleImputer
# invoking SimpleImputer to fill missing values
imputer = SimpleImputer(missing_values=np.nan, strategy='mean')
df[COLS] = imputer.fit_transform(df[COLS])
df['type'].value_counts()
df['type'].replace({"RRO":1, "I":2, "RO":3,"S":4,"RIRUO":5,"R":6}, inplace= True)
from sklearn.preprocessing import LabelEncoder
labelencoder=LabelEncoder()
df["state"]=labelencoder.fit_transform(df["state"])
df.head(5)
