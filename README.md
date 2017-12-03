# Weight-Manage-Program"""A program that takes in a new weight in pound each week from human user and calculate the change from the previous input. Then plot a graph of the weight chaning trend with x-axis as week and y-axis as weight"""

import plotly
plotly.tools.set_credentials_file(
    username='yinzhema', api_key='qRAg4ZUTBUsiOmcVgMGk')
import plotly.plotly as py
import plotly.graph_objs as go


#Return a dictionary with week number and weekly weight and weight change
def changing_weight():
    original_weight = float(raw_input('What is your original weight?'))
    week = 0
    weight_dict = {}
    new_weight = float(raw_input('How much do you weight this week?'))
    while new_weight != 0.0:
        weight_set = []
        weight_set.append(new_weight)
        week = week + 1
        changing_weight = new_weight - original_weight
        weight_set.append(changing_weight)
        weight_dict['Week ' + str(week)] = weight_set
        original_weight = new_weight
        new_weight = float(raw_input('How much do you weight this week?'))
    return weight_dict


weight_dict = changing_weight()


#create a list with weekly weights
def create_week_weight(weight_dict):
    weight_week_list = []
    for key in weight_dict:
        weekly_weight = weight_dict[key][0]
        weight_week_list.append(weekly_weight)
    return weight_week_list


weight_week_list = create_week_weight(weight_dict)


#find the minimum value in a given list
def minimum(weight_week_list):
    min_num = weight_week_list[0]
    for number in weight_week_list[1:]:
        if number < min_num:
            min_num = number
    return min_num


#produce a ordered weekly weight list
def ordered_list(weight_week_list):
    ordered_weight_list = []
    length0 = len(weight_week_list)
    weight_week_list1 = weight_week_list[:]
    for index in range(length0):
        min_weight = minimum(weight_week_list1)
        ordered_weight_list.append(min_weight)
        weight_week_list1.remove(min_weight)
    return ordered_weight_list


ordered_weight_list = ordered_list(weight_week_list)


#create a list with number from 1 to the number of week provided
def create_list(weight_dict):
    length = len(weight_dict.keys())
    list_week = []
    for number in range(1, length + 1):
        list_week.append('Week ' + str(number))
    return list_week


list_week = create_list(weight_dict)
#graphing Part
# def graph(list_week,ordered_weight_list):

trace0 = go.Scatter(
    x=list_week, y=ordered_weight_list, mode='markers', name='markers')
data = [trace0]
py.iplot(data, filename='yinzhema')


"""For the next stage of the program, it will ask the human user's weight and height, then it will go the BMI file to return the Body Mass Index"""


def bmi_calculator():
    age = int(raw_input('How old are you?'))
    if age >= 18:
        user_weight_unit = raw_input(
            'What is the unit for your weight?Please enter KG or P')
        user_weight = raw_input('How much do you weight?')
        user_height_unit = raw_input(
            'What is the unit for you height? Please enter M or FT')
        if user_height_unit == 'FT':
            user_height_ft = raw_input('What is your height--Feet?')
            user_height_inch = raw_input('What is your height--inch?')
            user_height_cm = (float(user_height_ft) * 30.48) + (
                float(user_height_inch) * 2.54)
            user_height_m = user_height_cm / 100.0
        if user_weight_unit == 'KG':
            body_mass_index = float(user_weight) / user_height_m**2
        elif user_weight_unit == 'P':
            user_weight_kg = float(user_weight) * 0.45
            body_mass_index = user_weight_kg / user_height_m**2
        return body_mass_index, user_height_m, user_weight_kg
    else:
        print 'Sorry, grow up kid!'
        return None


def bmi_analysis(body_mass_index):
    if body_mass_index < 18.5:
        message = 'in the underweight range.'
    elif body_mass_index >= 18.5 and body_mass_index < 25.0:
        message = 'in the Normal range.'
    elif body_mass_index >= 25.0 and body_mass_index < 29.9:
        message = 'in the overweight range.'
    else:
        message = 'in the obese range.'
    return message


"""This part takes in the dream weight from human user and return the plan"""


def set_goal():
    whether_set_goal = raw_input(
        'Do you want to set a goal for your Body weight? Enter Yes or No')
    if whether_set_goal == 'No':
        print 'Thanks for using this program!'
        return None
    else:
        dream_weight_unit = raw_input(
            'What is the unit for your dream weight?Please enter KG or P')
        dream_weight = raw_input('what is your dream weight')
        if dream_weight_unit == 'KG':
            weight_change = float(dream_weight) - dream_weight_unit
        elif dream_weight_unit == 'P':
            dream_weight = float(dream_weight) * 0.45
            weight_change = dream_weight - user_weight_kg
        time_left_unit = raw_input(
            'When do you want to acheive this goal?Pleas enter Mth or Day or Yr'
        )
        time_left = int(raw_input('When do you want to acheive this goal?'))
        if time_left_unit == 'Day':
            if time_left <= 7:
                avg_weight_change = weight_change / time_left
            elif time_left > 7:
                time_left_week = time_left / 7
                if time_left_week <= 1:
                    avg_weight_change = weight_change / time_left
                else:
                    if time_left % 7 >= 4:
                        avg_weight_change = weight_change / time_left_week + 1
                    else:
                        avg_weight_change = weight_change / time_left_week
        elif time_left_unit == 'Mth':
            time_week = time_left * 4
            avg_weight_change = weight_change / time_week
        elif time_left_unit == 'Yr':
            time_day = time_left * 365
            if time_day % 7 >= 4:
                avg_weight_change = weight_change / (time_day / 7) + 1
            else:
                avg_weight_change = weight_change / (time_day / 7)
        return avg_weight_change, dream_weight


def main2():
    if bmi_calculator() != None:
        body_mass_index, user_height_m, user_weight_kg = bmi_calculator()
        message = bmi_analysis(body_mass_index)
        print 'Since your height is ' + str(
            user_height_m) + ' and your weight is' + str(
                user_weight_kg) + ' Kg. Your Body Mass Index is' + str(
                    body_mass_index) + ', which is' + message
    return


def main3():
    if set_goal() != None:
        avg_weight_change, dream_weight = set_goal()
        print 'You need to change your weight ' + str(
            avg_weight_change
        ) + ' Kg each week in order to reach your dream weight:' + str(
            dream_weight) + ' Kg'
    return


def main1():
    weight_dict = changing_weight()
    weight_week_list = create_week_weight(weight_dict)
    ordered_weight_list = ordered_list(weight_week_list)
    list_week = create_list(weight_dict)
    import plotly
    plotly.tools.set_credentials_file(
        username='yinzhema', api_key='qRAg4ZUTBUsiOmcVgMGk')
    import plotly.plotly as py
    import plotly.graph_objs as go
    trace0 = go.Scatter(
        x=list_week, y=ordered_weight_list, mode='markers', name='markers')
    data = [trace0]
    return py.iplot(data, filename='weightgraph')


def main():
    which_part = raw_input(
        'Which part of the program do you want to use first? Part I or Part II? or Part III?'
    )
    if which_part == 'Part I':
        return main1()
    elif which_part == 'Part II':
        return main2()
    else:
        return main3()


main()
