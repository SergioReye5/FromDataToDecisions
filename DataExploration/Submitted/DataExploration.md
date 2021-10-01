# Data Exploration

## Data Cleaning

The code below was used to prepare the data to generate the plots.

```
d = subset(HFS.Dataset.Original, simple_race == "1"| simple_race == "16" |simple_race == "4"| simple_race == "64")
d$simple_race<-as.character(d$simple_race)
d$simple_race[d$simple_race=="1"]<-"Caucasian"
d$simple_race[d$simple_race=="16"]<-"Black"
d$simple_race[d$simple_race=="4"]<-"Native American"
d$simple_race[d$simple_race=="64"]<-"Two or More"
d$simple_race[d$ethnic_identity=="Mexican"]<-"Hispanic"
d$simple_race[d$ethnic_identity=="Other Hispanic or Latino"]<-"Hispanic"
d$TimetoApprove <- d$approved_date-d$date_entered
```

![Bar Graph](https://github.com/SergioReye5/FromDataToDecisions/blob/main/DataExploration/Submitted/Bar%20Graph.png)

```
ggplot(data = d, aes(x=simple_race, y = actual_date, fill = simple_race))+ stat_summary(fun = "mean", geom = "bar")+facet_grid(~program_name)+labs(fill = "Race", x= "Race", y = "Average Treatment Lenght")+theme(axis.title.x=element_blank(),axis.text.x=element_blank())

```

In this graph we can see the average time spent in treatment broken down by race and program. We can see that Native Americans spend longer in treatment than the rest. More exploration will be needed to determine if these are outliers. The time spent in the gambling program on Caucasian compared to African American also seems problematic.

![Facetted Plot](https://github.com/SergioReye5/FromDataToDecisions/blob/main/DataExploration/Submitted/Facetted%20Plot.png)

```
ggplot(data = d, aes(x=actual_date, y = total_duration_num))+ stat_summary(fun = "mean", geom = "point")+facet_wrap(~simple_race, nrow = 4)+labs(x= "Time in Treatment", y = "Session Lenght")
```

This plot's main goal was to try and explore if time spent in the program affects the session's length. Instead I found something else that caught my eye. Native Americans have long gaps of time between sessions. 

![Trend Line](https://github.com/SergioReye5/FromDataToDecisions/blob/main/DataExploration/Submitted/Bar%20Graph.png)

```
ggplot(data = d, aes(x=total_duration_num, y = TimetoApprove))+ stat_summary(fun = "mean", geom = "point")+ xlim(0, 300)+ geom_smooth(method = "lm")+labs(x="Session Length", y="Time To Approve")
```

This scatter plot shows us that the longer the sessions the longer it takes for them to be approved by a supervisor. Time to apporve is calculated by substracting the fields approved date and date entered. This plot might be out of scope for my group's research questions but I found it interesting.
