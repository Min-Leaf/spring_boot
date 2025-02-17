package com.example.nagoyameshi.form;

import jakarta.validation.constraints.NotBlank;
import jakarta.validation.constraints.NotNull;

import org.hibernate.validator.constraints.Length;

import lombok.Data;

// パスワード変更の機能
@Data
public class PasswordResetForm {
	@NotNull
    private Integer userId; 
	
	@NotBlank(message = "パスワードを入力してください。")
	@Length(min = 8, message = "パスワードは8文字以上で入力してください。")
	private String password; 
	
	@NotBlank(message = "パスワード（確認用）を入力してください。")
	private String passwordConfirmation; 
	
    // Getter, Setter
    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String getPasswordConfirmation() {
        return passwordConfirmation;
    }

    public void setPasswordConfirmation(String passwordConfirmation) {
        this.passwordConfirmation = passwordConfirmation;
    }

	public String getEmail() {
		return passwordConfirmation;
	}
}
