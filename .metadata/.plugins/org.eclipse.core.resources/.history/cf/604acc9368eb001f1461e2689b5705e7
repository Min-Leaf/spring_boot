package com.example.nagoyameshi.controller;

import java.time.format.DateTimeFormatter;

import org.springframework.security.core.annotation.AuthenticationPrincipal;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.validation.BindingResult;
import org.springframework.validation.FieldError;
import org.springframework.validation.annotation.Validated;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.mvc.support.RedirectAttributes;

import com.example.nagoyameshi.entity.User;
import com.example.nagoyameshi.form.PasswordResetForm;
import com.example.nagoyameshi.form.UserEditForm;
import com.example.nagoyameshi.security.UserDetailsImpl;
import com.example.nagoyameshi.service.UserService;

// 会員情報編集用フォーム
@Controller
@RequestMapping("/user")
public class UserController {
	private final UserService userService;

	public UserController(UserService userService) {
		this.userService = userService;

	}

	@GetMapping
	public String index(@AuthenticationPrincipal UserDetailsImpl userDetailsImpl, Model model) {
		User user = userDetailsImpl.getUser();

		model.addAttribute("user", user);

		return "user/index";
	}

	@GetMapping("/edit")
	public String edit(@AuthenticationPrincipal UserDetailsImpl userDetailsImpl, Model model) {
		User user = userDetailsImpl.getUser();
		String birthday = null;

		if (user.getBirthday() != null) {
			birthday = user.getBirthday().format(DateTimeFormatter.ofPattern("yyyyMMdd"));
		}

		UserEditForm userEditForm = new UserEditForm(user.getName(),
				user.getFurigana(),
				user.getPostalCode(),
				user.getAddress(),
				user.getPhoneNumber(),
				birthday,
				user.getOccupation(),
				user.getEmail());
		model.addAttribute("userEditForm", userEditForm);

		return "user/edit";
	}

	@PostMapping("/update")
	public String update(@ModelAttribute @Validated UserEditForm userEditForm,
			BindingResult bindingResult,
			@AuthenticationPrincipal UserDetailsImpl userDetailsImpl,
			RedirectAttributes redirectAttributes,
			Model model) {
		User user = userDetailsImpl.getUser();

		// メールアドレスが変更されており、かつ登録済みであれば、BindingResultオブジェクトにエラー内容を追加する
		if (userService.isEmailChanged(userEditForm, user) && userService.isEmailRegistered(userEditForm.getEmail())) {
			FieldError fieldError = new FieldError(bindingResult.getObjectName(), "email", "すでに登録済みのメールアドレスです。");
			bindingResult.addError(fieldError);
		}

		if (bindingResult.hasErrors()) {
			model.addAttribute("userEditForm", userEditForm);

			return "user/edit";
		}

		userService.updateUser(userEditForm, user);
		redirectAttributes.addFlashAttribute("successMessage", "会員情報を編集しました。");

		return "redirect:/user";
	}
	
	// パスワード編集フォームを表示
    @GetMapping("/edit-password")
    public String editPassword(@AuthenticationPrincipal UserDetailsImpl userDetailsImpl, Model model) {
    	PasswordResetForm userEditPasswordForm = new PasswordResetForm();
        model.addAttribute("userEditPasswordForm", userEditPasswordForm);
        return "user/edit_password";  // ここで表示するビューの名前を指定
    }

    // パスワード更新処理
    @PostMapping("/update-password")
    public String changePassword(@ModelAttribute @Validated PasswordResetForm userEditPasswordForm,
            BindingResult bindingResult, 
            @AuthenticationPrincipal UserDetailsImpl userDetailsImpl, 
            RedirectAttributes redirectAttributes) {

        // パスワード確認一致チェック
        if (!userEditPasswordForm.getPassword().equals(userEditPasswordForm.getPasswordConfirmation())) {
            bindingResult.rejectValue("passwordConfirmation", "error.passwordConfirmation", "パスワード確認が一致しません。");
        }

        if (bindingResult.hasErrors()) {
            return "user/edit_password";  // エラーがあれば編集フォームに戻す
        }

        // ユーザー情報を取得し、パスワードを更新
        User user = userDetailsImpl.getUser();
        user.setPassword(userEditPasswordForm.getPassword()); // パスワードは暗号化して保存する必要がある
        userService.updateUser(user);  // ここでパスワードを保存

        redirectAttributes.addFlashAttribute("successMessage", "パスワードが更新されました。");

        return "redirect:/user";  // 更新後にリダイレクト
    }
}