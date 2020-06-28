# Making Plot 3 - dateTime versus Energy sub metering

# Loads and adds the package "data.table"

library("data.table")

# Returns the current directory before the change, invisibly and with 
# the same conventions as getwd.

setwd("C:/Users/Pabricio Marcos/Desktop/Coursera/curso")

# Reads the data GAP (Global active power) from file then subsets data for specified 
# dates

GAP <- data.table::fread(input = "household_power_consumption.txt"
                             , na.strings="?"
)

# Prevents Scientific Notation

GAP[, Global_active_power := lapply(.SD, as.numeric), .SDcols = c("Global_active_power")]

# Making a POSIXct date capable of being filtered and graphed by time of day

GAP[, dateTime := as.POSIXct(paste(Date, Time), format = "%d/%m/%Y %H:%M:%S")]

# Filter Dates for 2007-02-01 and 2007-02-02

GAP <- GAP[(dateTime >= "2007-02-01") & (dateTime < "2007-02-03")]

png("plot3.png", width=480, height=480)

# Plot 3
plot(GAP[, dateTime], GAP[, Sub_metering_1], type="l", xlab="", ylab="Energy sub metering")
lines(GAP[, dateTime], GAP[, Sub_metering_2],col="red")
lines(GAP[, dateTime], GAP[, Sub_metering_3],col="blue")
legend("topright"
       , col=c("black","red","blue")
       , c("Sub_metering_1  ","Sub_metering_2  ", "Sub_metering_3  ")
       ,lty=c(1,1), lwd=c(1,1))

dev.off()
![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-4.png) 
