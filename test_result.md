#====================================================================================================
# START - Testing Protocol - DO NOT EDIT OR REMOVE THIS SECTION
#====================================================================================================

# THIS SECTION CONTAINS CRITICAL TESTING INSTRUCTIONS FOR BOTH AGENTS
# BOTH MAIN_AGENT AND TESTING_AGENT MUST PRESERVE THIS ENTIRE BLOCK

# Communication Protocol:
# If the `testing_agent` is available, main agent should delegate all testing tasks to it.
#
# You have access to a file called `test_result.md`. This file contains the complete testing state
# and history, and is the primary means of communication between main and the testing agent.
#
# Main and testing agents must follow this exact format to maintain testing data. 
# The testing data must be entered in yaml format Below is the data structure:
# 
## user_problem_statement: {problem_statement}
## backend:
##   - task: "Task name"
##     implemented: true
##     working: true  # or false or "NA"
##     file: "file_path.py"
##     stuck_count: 0
##     priority: "high"  # or "medium" or "low"
##     needs_retesting: false
##     status_history:
##         -working: true  # or false or "NA"
##         -agent: "main"  # or "testing" or "user"
##         -comment: "Detailed comment about status"
##
## frontend:
##   - task: "Task name"
##     implemented: true
##     working: true  # or false or "NA"
##     file: "file_path.js"
##     stuck_count: 0
##     priority: "high"  # or "medium" or "low"
##     needs_retesting: false
##     status_history:
##         -working: true  # or false or "NA"
##         -agent: "main"  # or "testing" or "user"
##         -comment: "Detailed comment about status"
##
## metadata:
##   created_by: "main_agent"
##   version: "1.0"
##   test_sequence: 0
##   run_ui: false
##
## test_plan:
##   current_focus:
##     - "Task name 1"
##     - "Task name 2"
##   stuck_tasks:
##     - "Task name with persistent issues"
##   test_all: false
##   test_priority: "high_first"  # or "sequential" or "stuck_first"
##
## agent_communication:
##     -agent: "main"  # or "testing" or "user"
##     -message: "Communication message between agents"

# Protocol Guidelines for Main agent
#
# 1. Update Test Result File Before Testing:
#    - Main agent must always update the `test_result.md` file before calling the testing agent
#    - Add implementation details to the status_history
#    - Set `needs_retesting` to true for tasks that need testing
#    - Update the `test_plan` section to guide testing priorities
#    - Add a message to `agent_communication` explaining what you've done
#
# 2. Incorporate User Feedback:
#    - When a user provides feedback that something is or isn't working, add this information to the relevant task's status_history
#    - Update the working status based on user feedback
#    - If a user reports an issue with a task that was marked as working, increment the stuck_count
#    - Whenever user reports issue in the app, if we have testing agent and task_result.md file so find the appropriate task for that and append in status_history of that task to contain the user concern and problem as well 
#
# 3. Track Stuck Tasks:
#    - Monitor which tasks have high stuck_count values or where you are fixing same issue again and again, analyze that when you read task_result.md
#    - For persistent issues, use websearch tool to find solutions
#    - Pay special attention to tasks in the stuck_tasks list
#    - When you fix an issue with a stuck task, don't reset the stuck_count until the testing agent confirms it's working
#
# 4. Provide Context to Testing Agent:
#    - When calling the testing agent, provide clear instructions about:
#      - Which tasks need testing (reference the test_plan)
#      - Any authentication details or configuration needed
#      - Specific test scenarios to focus on
#      - Any known issues or edge cases to verify
#
# 5. Call the testing agent with specific instructions referring to test_result.md
#
# IMPORTANT: Main agent must ALWAYS update test_result.md BEFORE calling the testing agent, as it relies on this file to understand what to test next.

#====================================================================================================
# END - Testing Protocol - DO NOT EDIT OR REMOVE THIS SECTION
#====================================================================================================



#====================================================================================================
# Testing Data - Main Agent and testing sub agent both should log testing data below this section
#====================================================================================================


user_problem_statement: |
  إصلاح شامل لنظام صيانة الهواتف "Baqerr" مع إضافة ميزات جديدة:
  1. إصلاح خطأ toLocaleString في صفحة الديون
  2. إصلاح حذف المهام في لوحة الإدارة
  3. إصلاح ميزة عرض الموقع
  4. التأكد من إضافة الديون التلقائية عند ترك رصيد على الزبون
  5. إنشاء نظام إدارة قطع الغيار (إضافة، تعديل، أرشفة)
  6. تحسين تصميم سجل الموظف/الوكيل (تصميم احترافي + تاريخ كبير وواضح)

backend:
  - task: "إصلاح API الديون - توحيد استخدام repair_cost"
    implemented: true
    working: "NA"
    file: "/app/almahel/backend/server.py (lines 1144-1160, 1395-1414)"
    stuck_count: 0
    priority: "high"
    needs_retesting: true
    status_history:
      - working: "NA"
        agent: "main"
        comment: "أصلحت الباك إند لاستخدام repair_cost بدلاً من total_amount في كل مكان. هذا يحل مشكلة البيانات undefined."

  - task: "إضافة الديون التلقائية عند إنشاء صيانة"
    implemented: true
    working: "NA"
    file: "/app/almahel/backend/server.py (lines 1144-1160)"
    stuck_count: 0
    priority: "high"
    needs_retesting: true
    status_history:
      - working: "NA"
        agent: "main"
        comment: "الكود موجود بالفعل. عند إضافة صيانة بدفع جزئي، يُنشأ دين تلقائياً في repair_debts collection."

  - task: "نظام قطع الغيار الكامل (Spare Parts API)"
    implemented: true
    working: "NA"
    file: "/app/almahel/backend/server.py (after line 1445)"
    stuck_count: 0
    priority: "high"
    needs_retesting: true
    status_history:
      - working: "NA"
        agent: "main"
        comment: |
          أضفت API كامل لقطع الغيار:
          - GET /api/spare-parts (القطع النشطة)
          - GET /api/spare-parts/archived (المؤرشف)
          - POST /api/spare-parts (إضافة قطعة)
          - PUT /api/spare-parts/{id} (تحديث)
          - DELETE /api/spare-parts/{id} (حذف نهائي)
          - PATCH /api/spare-parts/{id}/archive (أرشفة)
          - PATCH /api/spare-parts/{id}/unarchive (إلغاء الأرشفة)

  - task: "حذف المهام (Tasks)"
    implemented: true
    working: "NA"
    file: "/app/almahel/backend/server.py (lines 549-555)"
    stuck_count: 0
    priority: "high"
    needs_retesting: true
    status_history:
      - working: "NA"
        agent: "main"
        comment: "endpoint موجود ويبدو صحيحاً: DELETE /api/tasks/{task_id}"

frontend:
  - task: "إصلاح toLocaleString error في صفحة الديون"
    implemented: true
    working: "NA"
    file: "/app/almahel/frontend/src/pages/PhoneRepairSystemFull.js (lines 1206-1212, 1281-1288)"
    stuck_count: 2
    priority: "high"
    needs_retesting: true
    status_history:
      - working: false
        agent: "user"
        comment: "المستخدم أبلغ عن خطأ متكرر: Cannot read properties of undefined (reading 'toLocaleString')"
      - working: "NA"
        agent: "main"
        comment: |
          أضفت حماية للبيانات في filter و map:
          - التحقق من أن debt && debt.customer_name && debt.customer_phone && debt.phone_model موجودة
          - هذا يحمي من undefined objects قبل الوصول إلى properties

  - task: "حذف المهام في Admin Dashboard"
    implemented: true
    working: "NA"
    file: "/app/almahel/frontend/src/pages/AdminDashboard.js (lines 271-281)"
    stuck_count: 1
    priority: "high"
    needs_retesting: true
    status_history:
      - working: false
        agent: "user"
        comment: "المستخدم أبلغ أن حذف المهام لا يعمل"
      - working: "NA"
        agent: "main"
        comment: "الكود يبدو صحيحاً: يستدعي DELETE /api/tasks/{taskId}. يحتاج اختبار."

  - task: "نظام قطع الغيار الكامل - Frontend"
    implemented: true
    working: "NA"
    file: "/app/almahel/frontend/src/pages/PhoneRepairSystemFull.js (after line 1555)"
    stuck_count: 0
    priority: "high"
    needs_retesting: true
    status_history:
      - working: "NA"
        agent: "main"
        comment: |
          أضفت صفحة كاملة لأرشيف قطع الغيار مع:
          - Tab جديد في التنقل
          - عرض القطع النشطة والمؤرشفة
          - نموذج إضافة قطعة مع الحقول: موديل، اسم القطعة، السعر، الكمية، ملاحظات
          - أزرار تعديل، أرشفة، استرجاع، حذف نهائي
          - تصميم احترافي مع أيقونات ومعلومات واضحة

  - task: "تحسين تصميم صفحة الوكيل"
    implemented: true
    working: "NA"
    file: "/app/almahel/frontend/src/pages/TechnicianDashboard.js (lines 679-780)"
    stuck_count: 0
    priority: "medium"
    needs_retesting: true
    status_history:
      - working: "NA"
        agent: "main"
        comment: |
          حسّنت التصميم بشكل كامل:
          - تاريخ كبير وواضح في header كل مهمة (تاريخ الإنشاء + تاريخ الإنجاز)
          - تصميم احترافي مع gradients وألوان جذابة
          - بطاقات كبيرة مع معلومات منظمة
          - أزرار كبيرة وواضحة
          - عرض فارغ جميل عند عدم وجود مهام

metadata:
  created_by: "main_agent"
  version: "1.0"
  test_sequence: 1
  run_ui: true

test_plan:
  current_focus:
    - "إصلاح toLocaleString error في صفحة الديون"
    - "حذف المهام في Admin Dashboard"
    - "نظام قطع الغيار الكامل"
    - "الديون التلقائية"
    - "تحسين تصميم صفحة الوكيل"
  stuck_tasks:
    - "إصلاح toLocaleString error في صفحة الديون"
  test_all: false
  test_priority: "stuck_first"

agent_communication:
  - agent: "main"
    message: |
      قمت بالإصلاحات والتحسينات التالية:
      
      1. ✅ Backend: أصلحت API الديون لاستخدام repair_cost بشكل موحد
      2. ✅ Backend: أضفت نظام قطع الغيار الكامل مع 7 endpoints
      3. ✅ Frontend: أصلحت toLocaleString error بإضافة حماية للبيانات
      4. ✅ Frontend: أضفت صفحة قطع الغيار الكاملة مع أرشفة
      5. ✅ Frontend: حسّنت تصميم صفحة الوكيل بشكل كامل (تواريخ كبيرة + تصميم احترافي)
      
      الرجاء اختبار:
      - تسجيل الدخول كـ baqerr/11223300
      - الذهاب لصفحة "ديون الصيانة" والتحقق من عدم وجود أخطاء toLocaleString
      - إضافة صيانة جديدة بدفع جزئي والتحقق من إنشاء الدين تلقائياً
      - تجربة صفحة "أرشيف قطع الغيار" (إضافة، تعديل، أرشفة، استرجاع)
      - تسجيل الدخول كموظف والتحقق من التصميم الجديد
      - تسجيل الدخول كـ admin/198212 وتجربة حذف مهمة
      
      credentials:
      - نظام المهام: admin / 198212
      - نظام الصيانة: baqerr / 11223300
      - موظف: (إن وُجد في النظام)
