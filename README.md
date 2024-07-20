# Spring Boot Annotations

## Annotations Table

| Annotation       | Description                                                                 |
|------------------|-----------------------------------------------------------------------------|
| @RestController  | Marks the class as a RESTful web service controller.                        |
| @RequestMapping  | Maps HTTP requests to handler methods of MVC and REST controllers.          |
| @GetMapping      | Handles HTTP GET requests.                                                  |
| @PathVariable    | Binds a method parameter to a URI template variable.                        |
| @RequestParam    | Binds a method parameter to a web request parameter.                        |
| @PostMapping     | Handles HTTP POST requests.                                                 |
| @RequestBody     | Binds the body of the web request to a method parameter.                    |
| @PutMapping      | Handles HTTP PUT requests.                                                  |
| @DeleteMapping   | Handles HTTP DELETE requests.                                               |

```java


// @RestController
@RequestMapping("students")  // Map HTTP requests to /students
public class StudentController {

    // @GetMapping for GET request at /students/student
    // http://localhost:8080/students/student
    @GetMapping("student")
    public ResponseEntity<Student> getStudent(){
        Student student = new Student(
          1,
          "Ramesh",
          "Fadatare"
        );
        // Return a response with status OK and custom header
        return ResponseEntity.ok()
                .header("custom-header", "ramesh")
                .body(student);
    }

    // @GetMapping for GET request at /students
    // http://localhost:8080/students
    @GetMapping
    public ResponseEntity<List<Student>> getStudents(){
        List<Student> students = new ArrayList<>();
        students.add(new Student(1, "Ramesh", "Fadatare"));
        students.add(new Student(2, "umesh", "Fadatare"));
        students.add(new Student(3, "Ram", "Jadhav"));
        students.add(new Student(4, "Sanjay", "Pawar"));
        // Return a list of students with status OK
        return ResponseEntity.ok(students);
    }

    // @GetMapping for GET request with path variables
    // http://localhost:8080/students/1/ramesh/fadatare
    @GetMapping("{id}/{first-name}/{last-name}")
    public ResponseEntity<Student> studentPathVariable(@PathVariable("id") int studentId,
                                       @PathVariable("first-name") String firstName,
                                       @PathVariable("last-name") String lastName){
        Student student = new Student(studentId, firstName, lastName);
        // Return a student with status OK
        return ResponseEntity.ok(student);
    }

    // @GetMapping for GET request with query parameters
    // http://localhost:8080/students/query?id=1&firstName=Ramesh&lastName=Fadatare
    @GetMapping("query")
    public ResponseEntity<Student> studentRequestVariable(@RequestParam int id,
                                          @RequestParam String firstName,
                                          @RequestParam String lastName){
        Student student = new Student(id, firstName, lastName);
        // Return a student with status OK
        return ResponseEntity.ok(student);
    }

    // @PostMapping to create a new student
    // @PostMapping and @RequestBody
    @PostMapping("create")
    //@ResponseStatus(HttpStatus.CREATED)
    public ResponseEntity<Student> createStudent(@RequestBody Student student){
        System.out.println(student.getId());
        System.out.println(student.getFirstName());
        System.out.println(student.getLastName());
        // Return the created student with status CREATED
        return new ResponseEntity<>(student, HttpStatus.CREATED);
    }

    // @PutMapping to update an existing student
    @PutMapping("{id}/update")
    public ResponseEntity<Student> updateStudent(@RequestBody Student student, @PathVariable("id") int studentId){
        System.out.println(student.getFirstName());
        System.out.println(student.getLastName());
        // Return the updated student with status OK
        return ResponseEntity.ok(student);
    }

    // @DeleteMapping to delete a student
    @DeleteMapping("{id}/delete")
    public ResponseEntity<String> deleteStudent(@PathVariable("id") int studentId){
        System.out.println(studentId);
        return ResponseEntity.ok("Student deleted successfully!");
    }
}
