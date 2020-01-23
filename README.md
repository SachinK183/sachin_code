# sachin_code
package com.app.controller;

import javax.annotation.PostConstruct;
import javax.servlet.http.HttpSession;
import javax.validation.Valid;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.validation.BindingResult;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.mvc.support.RedirectAttributes;

import com.app.dao.StudentDao;
import com.app.pojos.Student;
import com.app.service.StudentService;

@Controller
@RequestMapping("/Student")
public class StudentController {

	@Autowired
	private StudentService service;
	
	@PostConstruct
	public void init()
	{
		System.out.println("in init");
	}
	
	@GetMapping(value="/register")
	public String showRegForm(Student s,Model map)
	{
		map.addAttribute("Student", new Student());
		System.out.println("in show reg form");
		return "/Student/register";
	}
	
	@PostMapping(value="/register")
	public String processRegForm(RedirectAttributes attrs,@Valid Student s1,BindingResult res,Model map)
	{
		map.addAttribute("Student", new Student());
		System.out.println("in process reg form");
		if(res.hasErrors())
		{
			System.out.println("error");
			return "/Student/register";
		}
		attrs.addFlashAttribute("status",service.RegisterStudent(s1));
		return "redirect:/Student/registerSuccess";
	}
	
	@GetMapping(value="/Login")
	public String showLoginForm(Student s,Model map)
	{

		map.addAttribute("Student", new Student());
       // model.addAttribute("bulletin", new Bulletin());
		System.out.println("in show form");
		return "/Student/login1";
	}
	
	@PostMapping(value="/Login")
	public String processLoginForm(@Valid Student s,BindingResult res,RedirectAttributes attrs,HttpSession hs)
	{
		if(res.hasFieldErrors("MailId")||res.hasFieldErrors("Password"))
		{
			System.out.println("error in login");
			return "/Student/Login";
		}
		System.out.println("in process");
		String sts=service.Login(s.getMailId(), s.getPassword());
		if(sts.contains("fail"))
		{
			attrs.addAttribute("status",sts);
			return "/Student/Login";
		}
		hs.setAttribute("status", sts);
		return "redirect:/Student/Valid";
	}
	
	
	
	
	
}
