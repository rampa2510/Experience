# Optimisation
   1) Use Preact for smaller projects
   2) Use code splitting feature 
   3) Use tree shaking feature of webpack
   4) Use webpack magic comments
   5) Always use hooks such as useCallback,memo,useMemo
   6) Use fullname imports rather than tree destructuring imports
   7) useRef for data that when changed should not cause rerender example if you want to create a searchable list of students then you an store all student data in useRef and in the state you can store the data of students you want to store this will benefit us as we can then perform all operations on useRef like slicing delete etc and the state will not be changed and if want to search for a student we can slice from the useRef and store it in state
   
# Coding style
   1) Use hooks instead of class
   2) Follow container design pattern
   3) Use services

 