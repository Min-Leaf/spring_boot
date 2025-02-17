package com.example.nagoyameshi.controller;

import jakarta.servlet.http.HttpServletRequest;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.validation.BindingResult;
import org.springframework.validation.FieldError;
import org.springframework.validation.annotation.Validated;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.servlet.mvc.support.RedirectAttributes;

import com.example.nagoyameshi.event.SignupEventPublisher;
import com.example.nagoyameshi.form.PasswordResetForm;
import com.example.nagoyameshi.service.PasswordResetService;
import com.example.nagoyameshi.service.UserService;
import com.example.nagoyameshi.service.VerificationTokenService;

@Controller
public class PasswordResetController {
	private UserService userService;
	   // メール認証機能
	   private SignupEventPublisher signupEventPublisher;
	   private VerificationTokenService verificationTokenService;

	   public PasswordResetController(UserService userService, SignupEventPublisher signupEventPublisher, VerificationTokenService verificationTokenService) {
	       this.userService = userService;
	       this.signupEventPublisher = signupEventPublisher;
	       this.verificationTokenService = verificationTokenService;
	   }
	

    @Autowired
    private PasswordResetService passwordResetService;

    // パスワード再設定ページの表示
    @GetMapping("/password/sendmail")
    public String showPasswordResetPage(Model model) {
        model.addAttribute("passwordResetForm", new PasswordResetForm());
        return "password/sendmail";  // sendmail.htmlを表示
    }

    // パスワードリセット用のメール送信
    @PostMapping("/reset")
    public String resetPassword(@ModelAttribute @Validated PasswordResetForm passwordResetForm,
                                 BindingResult bindingResult,
                                 RedirectAttributes redirectAttributes,
                                 HttpServletRequest httpServletRequest, // メール認証機能
                                 Model model) {
        // メールアドレスが登録されていない場合、エラーを追加する
        if (!userService.isEmailRegistered(passwordResetForm.getEmail())) {
            FieldError fieldError = new FieldError(bindingResult.getObjectName(), "email", "登録されていないメールアドレスです。");
            bindingResult.addError(fieldError);
        }

        // メールアドレスが不正な形式の場合、エラーを追加する
        if (!userService.isValidEmail(passwordResetForm.getEmail())) {
            FieldError fieldError = new FieldError(bindingResult.getObjectName(), "email", "無効なメールアドレスです。");
            bindingResult.addError(fieldError);
        }

        if (bindingResult.hasErrors()) {
            model.addAttribute("passwordResetForm", passwordResetForm);
            return "/password/reset"; // エラーがあればパスワードリセットフォームに戻る
        }

        // パスワードリセット用のメール送信処理
        boolean sent = passwordResetService.sendPasswordResetMail(passwordResetForm.getEmail());

        // メール送信が成功した場合
        if (sent) {
            redirectAttributes.addFlashAttribute("successMessage", "ご入力いただいたメールアドレスにパスワードリセットのメールを送信しました。メールに記載されているリンクをクリックし、パスワードをリセットしてください。");
            return "redirect:/";  // 成功した場合、成功ページへリダイレクト
        } else {
            redirectAttributes.addFlashAttribute("errorMessage", "パスワードリセットのメール送信に失敗しました。もう一度お試しください。");
            return "redirect:/password/sendmail";   // 失敗した場合、エラーページへリダイレクト
        }
    }
}
