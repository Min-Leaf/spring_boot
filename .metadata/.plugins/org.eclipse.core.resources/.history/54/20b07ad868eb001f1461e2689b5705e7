package com.example.nagoyameshi.event;

import org.springframework.context.ApplicationEventPublisher;
import org.springframework.stereotype.Component;

import com.example.nagoyameshi.entity.User;

// メール認証用のイベント
@Component
public class SignupEventPublisher {
   private final ApplicationEventPublisher applicationEventPublisher;

   public SignupEventPublisher(ApplicationEventPublisher applicationEventPublisher) {
       this.applicationEventPublisher = applicationEventPublisher;
   }

   public void publishSignupEvent(User user, String requestUrl) {
       applicationEventPublisher.publishEvent(new SignupEvent(this, user, requestUrl));
   }

   // パスワード変更の機能
	public void publishPasswordResetEvent(User user, String requestUrl) {
		applicationEventPublisher.publishEvent(new PasswordResetEvent(this, user, requestUrl));
	}
}