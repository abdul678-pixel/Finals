use hospital_portal;


CREATE TABLE patients (
    patient_id INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    patient_name VARCHAR(45) NOT NULL,
    age INT NOT NULL,
    admission_date DATE,
    discharge_date DATE
);

	insert into patients (patient_name, age, admission_date, discharge_date)
values ("Abdoul", 24, "2023-01-21", "2023-10-23"), ("Fosuah", 56, "2023-08-21", "2023-11-23"), ("Kwakyewaa", 64, "2023-02-25", "2023-05-26");

select * from patients;

CREATE TABLE appointments (
    appointment_id INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    patient_id INT NOT NULL,
    doctor_id INT NOT NULL,
    appointment_date DATE NOT NULL,
    appointment_time TIME NOT NULL,  # Change the data type here
    FOREIGN KEY (patient_id) REFERENCES patients(patient_id),
    FOREIGN KEY (doctor_id) REFERENCES doctors(doctor_id)
);

select * from appointments;

create procedure appointmentbuilder (
	in patientappoint_id INT,
    in doctorappoint_id INT,
    in appointbuilderdate date,
    in appointbuildertime time
    
    )

begin select appointments ( patient_id, doctor_id, appointment_date, appointment_time)
values (patientappoint_id, doctorappoint_id, appointbuilderdate, appointbuildertime)

end //
 delimiter ;
 
 create procedure PatientDismissal(
  in patientappoint_id int
 )
 
 begin update patients
 set discharge_date = current_date() where patient_id = patientappoint_id
 end //
 
 delimiter ;
 CREATE TABLE doctors (
    doctor_id INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    doctor_name VARCHAR(60) NOT NULL,
    doctor_specialization VARCHAR(60) NOT NULL
);
INSERT INTO doctors (doctor_name, doctor_specialization)
values ("Dwayne Johnson",  "Podiatrist"), ("Eric Mann",  "Physician"), ("Graace John",  "Cardiologist");


select * from doctors;

CREATE VIEW doctor_appointment_patient_view AS
SELECT
    d.doctor_id,
    d.doctor_name,
    d.doctor_specialization,
    a.appointment_id,
    a.appointment_date,
    a.patient_id,
    p.patient_name
FROM
    doctors d
JOIN
    appointments a ON d.doctor_id = a.doctor_id
JOIN
    patients p ON a.patient_id = p.patient_id;
    
select * from doctor_appointment_patient_view
