package com.example.nagoyameshi.service;

import org.springframework.mail.SimpleMailMessage;
import org.springframework.mail.javamail.JavaMailSender;
import org.springframework.stereotype.Service;

@Service
public class PasswordResetService {

    private final JavaMailSender mailSender;

    public PasswordResetService(JavaMailSender mailSender) {
        this.mailSender = mailSender;
    }

    public boolean sendPasswordResetMail(String email) {
        // メールの作成
        SimpleMailMessage message = new SimpleMailMessage();
        message.setTo(email);
        message.setSubject("パスワード再設定のご案内");
        message.setText("以下のリンクからパスワードの再設定を行ってください。\n\n" +
                        "http://yourdomain.com/reset-password?token=exampleToken"); // リセットURLを設定

        try {
            mailSender.send(message);
            return true;
        } catch (Exception e) {
            return false;
        }
    }
}
