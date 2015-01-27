# Coursera
rankhospital <- function(state, outcome, num = "best") {

    full_data <- read.csv("outcome-of-care-measures.csv", colClasses="character")

    

   	column <- if (outcome == "heart attack") {

		"Hospital.30.Day.Death..Mortality..Rates.from.Heart.Attack"

	} else if (outcome == "heart failure") {

		"Hospital.30.Day.Death..Mortality..Rates.from.Heart.Failure"

	} else if (outcome == "pneumonia") {

		"Hospital.30.Day.Death..Mortality..Rates.from.Pneumonia"

	} else {

		stop("invalid outcome")

	}




	data_for_state <- full_data[full_data$State == state, c("Hospital.Name", column)]

	

	if (nrow(data_for_state) == 0) {

		stop("invalid state")	

	}




    data_for_state[,2] <- as.numeric(data_for_state[,2])

	ordered_data_for_state <- order(data_for_state[column], data_for_state$Hospital.Name, na.last=NA)

    

    if (num == "best") {

        as.character(data_for_state$Hospital.Name[ordered_data_for_state[1]])

    } else if (num == "worst") {

       as.character(data_for_state$Hospital.Name[ordered_data_for_state[length(ordered_data_for_state)]])

    } else if (is.numeric(num)) {

       as.character(data_for_state$Hospital.Name[ordered_data_for_state[num]])

    } else {

        stop("invalid num")

    }

}

best <- function(state, outcome) {
          data <- read.csv("outcome-of-care-measures.csv", colClasses="character")
          if (!outcome %in% c("heart attack", "heart failure", "pneumonia")) {

        stop("invalid outcome")

    } 
     index <- ifelse(outcome == "heart attack", 11, ifelse(outcome == "heart failure", 17, 23))

    
data[,index] <- suppressWarnings(as.numeric(data[,index]))

    data <- na.omit(data)
      

    states <- table(data$State)

    if (!state %in% names(states)) { 

        stop("invalid state")

    }

    

    

    slice <- subset(data, State==state)

    slice <- slice[order(slice[,index], na.last=TRUE),2]

    slice <- na.omit(slice)

    



    slice[1]


} 

