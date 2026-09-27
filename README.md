# ProjectManagementSystem

import java.io.File;
import java.util.ArrayList;
import java.util.List;

public class ProjectManagementSystem {


    static class Teacher {
        private int id;
        private String name;
        private String designation;
        private String department;
        private File signature;

        public Teacher(int id, String name, String designation, String department, File signature) {
            this.id = id;
            this.name = name;
            this.designation = designation;
            this.department = department;
            this.signature = signature;
        }

        public int getId() { return id; }
        public void setId(int id) { this.id = id; }

        public String getName() { return name; }
        public void setName(String name) { this.name = name; }

        public String getDesignation() { return designation; }
        public void setDesignation(String designation) { this.designation = designation; }

        public String getDepartment() { return department; }
        public void setDepartment(String department) { this.department = department; }

        public File getSignature() { return signature; }
        public void setSignature(File signature) { this.signature = signature; }

        @Override
        public String toString() {
            return "Teacher{id=" + id + ", name='" + name + "', designation='" + designation
                    + "', department='" + department + "'}";
        }
    }

    static class Student {
        private String rollNo;
        private String name;
        private String emailId;
        private long mobileNo;
        private File signature;

        public Student(String rollNo, String name, String emailId, long mobileNo, File signature) {
            this.rollNo = rollNo;
            this.name = name;
            this.emailId = emailId;
            this.mobileNo = mobileNo;
            this.signature = signature;
        }

        public String getRollNo() { return rollNo; }
        public void setRollNo(String rollNo) { this.rollNo = rollNo; }

        public String getName() { return name; }
        public void setName(String name) { this.name = name; }

        public String getEmailId() { return emailId; }
        public void setEmailId(String emailId) { this.emailId = emailId; }

        public long getMobileNo() { return mobileNo; }
        public void setMobileNo(long mobileNo) { this.mobileNo = mobileNo; }

        public File getSignature() { return signature; }
        public void setSignature(File signature) { this.signature = signature; }

        @Override
        public String toString() {
            return "Student{rollNo='" + rollNo + "', name='" + name + "', emailId='" + emailId + "'}";
        }

        @Override
        public boolean equals(Object o) {
            if (this == o) return true;
            if (!(o instanceof Student)) return false;
            Student student = (Student) o;
            return rollNo.equals(student.rollNo);
        }

        @Override
        public int hashCode() {
            return rollNo.hashCode();
        }
    }

    static class Project {
        private String code;
        private String title;

        public Project(String code, String title) {
            this.code = code;
            this.title = title;
        }

        public String getCode() { return code; }
        public void setCode(String code) { this.code = code; }

        public String getTitle() { return title; }
        public void setTitle(String title) { this.title = title; }

        @Override
        public String toString() {
            return "Project{code='" + code + "', title='" + title + "'}";
        }
    }

    static class Team {
        private static final int MIN_MEMBERS = 3;
        private static final int MAX_MEMBERS = 5;

        private String id;
        private Teacher supervisor;   // 1 Team -> 1 Teacher (a Teacher may supervise 0..9 Teams)
        private Student leader;       // 1 Team -> 1 Student (leads)
        private final List<Student> members = new ArrayList<>(); // 3..5 Students
        private Project project;      // 1 Team -> 1 Project (works on)

        public Team(String id) {
            this.id = id;
        }

        public String getId() { return id; }
        public void setId(String id) { this.id = id; }

        public Teacher getSupervisor() { return supervisor; }
        public Student getLeader() { return leader; }
        public List<Student> getMembers() { return new ArrayList<>(members); }
        public Project getProject() { return project; }
        public void setProject(Project project) { this.project = project; }

        /** Adds a student to the team (max 5 members). */
        public boolean addMember(Student student) {
            if (student == null) {
                System.out.println("Cannot add a null student.");
                return false;
            }
            if (members.size() >= MAX_MEMBERS) {
                System.out.println("Cannot add " + student.getName() + ": team already has the maximum of "
                        + MAX_MEMBERS + " members.");
                return false;
            }
            if (members.contains(student)) {
                System.out.println(student.getName() + " is already a member of this team.");
                return false;
            }
            members.add(student);
            System.out.println(student.getName() + " added to team " + id + ".");
            return true;
        }

        /** Removes a student from the team (must keep at least 3 members). */
        public boolean removeMember(Student student) {
            if (student == null || !members.contains(student)) {
                System.out.println("Cannot remove: student is not a member of this team.");
                return false;
            }
            if (members.size() <= MIN_MEMBERS) {
                System.out.println("Cannot remove " + student.getName() + ": team must have at least "
                        + MIN_MEMBERS + " members.");
                return false;
            }
            if (student.equals(leader)) {
                System.out.println("Cannot remove " + student.getName() + ": student is the current leader. "
                        + "Assign a new leader first.");
                return false;
            }
            members.remove(student);
            System.out.println(student.getName() + " removed from team " + id + ".");
            return true;
        }

        /** Assigns the supervising Teacher for this team. */
        public void assignSupervisor(Teacher supervisor) {
            this.supervisor = supervisor;
            System.out.println(supervisor != null
                    ? supervisor.getName() + " assigned as supervisor of team " + id + "."
                    : "Supervisor cleared for team " + id + ".");
        }

        /** Sets the leader of the team (must already be a member). */
        public boolean setLeader(Student leader) {
            if (leader == null) {
                System.out.println("Cannot set a null leader.");
                return false;
            }
            if (!members.contains(leader)) {
                System.out.println(leader.getName() + " must be a member of the team before becoming leader.");
                return false;
            }
            this.leader = leader;
            System.out.println(leader.getName() + " set as leader of team " + id + ".");
            return true;
        }

        @Override
        public String toString() {
            return "Team{id='" + id + "', supervisor=" + (supervisor != null ? supervisor.getName() : "none")
                    + ", leader=" + (leader != null ? leader.getName() : "none")
                    + ", members=" + members.size()
                    + ", project=" + (project != null ? project.getTitle() : "none") + "}";
        }
    }

    public static void main(String[] args) {
        Teacher teacher = new Teacher(1, "Dr. Sharma", "Associate Professor", "Computer Science",
                new File("sharma_signature.png"));

        Student s1 = new Student("MIT001", "Aarav", "aarav@example.com", 9876543210L,
                new File("aarav_sign.png"));
        Student s2 = new Student("MIT002", "Diya", "diya@example.com", 9876543211L,
                new File("diya_sign.png"));
        Student s3 = new Student("MIT003", "Kabir", "kabir@example.com", 9876543212L,
                new File("kabir_sign.png"));
        Student s4 = new Student("MIT004", "Anaya", "anaya@example.com", 9876543213L,
                new File("anaya_sign.png"));

        Project project = new Project("PRJ101", "Project Management System");

        Team team = new Team("T1");
        team.addMember(s1);
        team.addMember(s2);
        team.addMember(s3);
        team.addMember(s4);

        team.assignSupervisor(teacher);
        team.setLeader(s1);
        team.setProject(project);

        System.out.println();
        System.out.println(team);

        System.out.println();
        team.removeMember(s2);
        team.removeMember(s3);
        team.removeMember(s4); // fails: would drop below MIN_MEMBERS (3)
    }
}
