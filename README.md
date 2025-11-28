package com.college.Student.Controller;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.*;

import com.college.Student.Entity.*;
import com.college.Student.Repository.*;

@Controller
@RequestMapping("/")

public class StudentController {
	@Autowired
    private StudentRepository userRepository;

    @GetMapping
    public String index() {
        return "index";
    }

    @GetMapping("/user")
    public String getUser(@RequestParam String firstname, Model model) {
            Student user = userRepository.findByFirstname(firstname);
            if (user != null) {
                model.addAttribute("firstname", user.getFirstname());
                model.addAttribute("lastname", user.getLastname());
                model.addAttribute("message", "User found successfully!");
          
            }return "Welcome";
    }

    @PostMapping("/register")
    public String createUser(@ModelAttribute Student user, Model model) {
            userRepository.save(user);
            model.addAttribute("firstname", user.getFirstname());
            model.addAttribute("lastname", user.getLastname());
            model.addAttribute("message", "User created successfully!");
        
        return "Welcome";
    }

    @PostMapping("/user/update")
    public String updateUser(@RequestParam Long id, @ModelAttribute Student user, Model model) {
        
            Student existingUser = userRepository.findById(id)
                .orElseThrow(() -> new RuntimeException("User not found"));
            existingUser.setFirstname(user.getFirstname());
            existingUser.setLastname(user.getLastname());
            userRepository.save(existingUser);
            model.addAttribute("firstname", existingUser.getFirstname());
            model.addAttribute("lastname", existingUser.getLastname());
            model.addAttribute("message", "User updated successfully!");
        
        return "Welcome";
    }

    @PostMapping("/user/delete")
    public String deleteUser(@RequestParam Long id, Model model) {
            userRepository.deleteById(id);
            model.addAttribute("message", "User deleted successfully!");
       
        return "index";
    }
}

