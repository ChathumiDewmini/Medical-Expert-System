% Select the disease 
check :-
    	writeln("Hi! Welcome to the MD system."),
    	writeln("Please select the disease you want to diagnose: "),
    	writeln("1. Heart Attack"),
    	writeln("2. High Blood Pressure"),
    	writeln("3. Migraine"),
    	writeln("4. Stroke"),

% Loop until a valid input is received
	repeat,
    		read(Disease),
    		(between(1, 4, Disease)
			->  diagnosis(Disease)
    		;   
		writeln("Invalid input. Please enter a number between 1 to 4.")
    		).

% Heart Attack
diagnosis(1) :-
    writeln("Please answer the following questions with y or n."),
    write('Do you have chest pain? '),
    read(Ans1),
    write('Are you feeling shortness of breath? '),
    read(Ans2),
    write('Are you sweating? '),
    read(Ans3),
    write('Are you feeling nauseous? '),
    read(Ans4),
    write('Are you feeling fatigued? '),
    read(Ans5),

	% Medication
    	(Ans1 == y -> 
        writeln('You have a high chance of having a heart attack. Please go to the hospital immediately.'), nl
    ; 
	Ans2 == y -> 
        writeln('You have a high chance of a respiratory or cardiovascular problem. Please go to the hospital immediately.'), nl
    ; 
	
	countSymptoms([Ans1, Ans2, Ans3, Ans4, Ans5], NumSymptoms),
      (NumSymptoms >= 3 ->
          writeln('You might be having a heart attack.'),
          writeln('Please take the following medications: aspirin and nitroglycerin.')
      ; 
	NumSymptoms >= 2 ->
          writeln('You might be having a heart attack.'),
          writeln('Please take the following medications: aspirin.')
      ; 
	NumSymptoms == 1 ->
          writeln('There may be less chance of having a heart attack.'),
          writeln('Please take the following medications: nitroglycerin.')
      ; 
          writeln('You do not have a high chance of having a heart attack.'), nl
      )
    ).

% High Blood Pressure
diagnosis(2) :-
    write('Please enter your systolic blood pressure (top number): '),
    read(Systolic),
    write('Please enter your diastolic blood pressure (bottom number): '),
    read(Diastolic),
    calculateBloodPressureCategory(Systolic, Diastolic, Category),
    write('Your blood pressure category is: '),
    write(Category), nl,

	% If the pressure is high
    	(Category == 'Hypertensive Crisis' ->
        writeln('Your blood pressure is dangerously high. Please seek immediate medical attention.'), nl,
        diagnosis(1)
    ; 
	  Category == 'Stage 2 Hypertension' ->
	  writeln('Please take the following medications: amlodipine.'),
        writeln('Your blood pressure is high. Checking for symptoms of a heart attack...'), nl,
        diagnosis(1)
    ; 
	  Category == 'Stage 1 Hypertension' ->
	  writeln('Please take the following medications: Calcium Channel Blockers (CCB) .'),
        writeln('Your blood pressure is high. Checking for symptoms of a heart attack...'), nl,
        diagnosis(1)
    ;
	  Category == 'Elevated' ->
	  writeln('Please take the following medications:thiazide diuretic .')
    ;
        true
    ).

		% Calculating Pressure and the Category
		calculateBloodPressureCategory(Systolic, Diastolic, Category) :-
    		MeanPressure is Diastolic + (Systolic - Diastolic) / 3.0,
    		(Systolic >= 180, Diastolic >= 120 ->
        	Category = 'Hypertensive Crisis'
		; 
		Systolic >= 140, Diastolic >= 90 ->
        		Category = 'Stage 2 Hypertension'
    		; 
		Systolic >= 130, Diastolic >= 80 ->
        		Category = 'Stage 1 Hypertension'
    		; 
		Systolic >= 120 ->
        		Category = 'Elevated'
    		; 
        		Category = 'Normal'
    		),
    write('Your mean arterial pressure is '),
    write(MeanPressure),
    writeln(' mmHg.'). 

%Migraine
diagnosis(3) :-
    write('Please answer the following questions with y or n.'), nl,
    write('Have you experienced a headache in the past 24 hours? '),
    read(Ans1),
    write('Do you feel a pulsing or throbbing sensation on one side of your head? '),
    read(Ans2),
    write('Do you experience sensitivity to light or sound during your headaches? '),
    read(Ans3),
    write('Have you experienced nausea or vomiting during your headaches? '),
    read(Ans4),
    write('Have you experienced visual disturbances, such as seeing spots or flashing lights, during your headaches? '),
    read(Ans5),
    
    countSymptoms([Ans1, Ans2, Ans3, Ans4, Ans5], NumSymptoms),
    (
        NumSymptoms >= 4 ->
            writeln('You might be having migraine.'),
            writeln('Please take the following medications: triptan.')
        ; NumSymptoms >= 2 ->
            writeln('You might be having mild migraine.'),
            writeln('Please take the following medications:tylenol.')
        ; NumSymptoms == 1 ->
            writeln('You have a headache.'),
            writeln('Please take the following medications: paracetamol.')
        ; 
            writeln('You do not have a high chance of having a migraine.'), nl
    ). 

%Stroke
diagnosis(4) :-

    write('Please answer the following questions with y or n.'), nl,
    write('Do you have numbeness or weakness? '),
    read(Ans1),
    write('Do you have confusion or trouble speaking? '),
    read(Ans2),
    write('Do you have vision problems? '),
    read(Ans3),
    write('Do you have dizziness or loss of balance? '),
    read(Ans4),
    write('Do you have severe headache? '),
    read(Ans5),
    
    countSymptoms([Ans1, Ans2, Ans3, Ans4, Ans5], NumSymptoms),
    (
        NumSymptoms >= 4 ->
            writeln('You might be having stroke.'),
            writeln('Please go to the hospital.')
        ; NumSymptoms >= 2 ->
            writeln('You might be having stroke.'),
            writeln('Please take the following medications:asprin.')
        ; NumSymptoms == 1 ->
            writeln('You are not having a stroke.'),
            writeln('However, please take the following medications: paracetamol.')
        ; 
            writeln('You are not having a stroke.'), nl
    ).


%Counting Symptoms
countSymptoms([], 0).
countSymptoms([y | Rest], Count) :-
    countSymptoms(Rest, RestCount),
    Count is RestCount + 1.
countSymptoms([_ | Rest], Count) :-
    countSymptoms(Rest, Count).
